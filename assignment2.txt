from sim_parameters import TRANSITION_PROBS,HOLDING_TIMES
import numpy as np
import pandas as pd
import helper

def sample_creation_corrected(sample_ratio, countries):
    df = pd.read_csv('a2-countries.csv')

    col_list = ['person_id', 'age_group_name', 'country']
    sample_population = pd.DataFrame(columns=col_list)

    person_id = 0
    for country in countries:
        sample_sizes = dict()
        population = df[df['country'] == country]['population'].values[0]
        sample_size = population / sample_ratio
        sample_sizes['less_5']=(df[df['country']==country]['less_5'].values[0])*sample_size / 100
        sample_sizes['5_to_14'] = (df[df['country'] == country]['5_to_14'].values[0]) * sample_size / 100
        sample_sizes['15_to_24'] = (df[df['country'] == country]['15_to_24'].values[0]) * sample_size / 100
        sample_sizes['25_to_64'] = (df[df['country'] == country]['25_to_64'].values[0]) * sample_size / 100
        sample_sizes['over_65'] = (df[df['country'] == country]['over_65'].values[0]) * sample_size / 100

        distinct_list = []

        for age_group, value in sample_sizes.items():
            for _ in range(int(value)):
                person_id += 1
                distinct_dict = {
                    'person_id': person_id,
                    'age_group_name': age_group,
                    'country': country
                }
                distinct_list.append(distinct_dict)

        distinct_df = pd.DataFrame(distinct_list)
        sample_population = pd.concat([sample_population, distinct_df], ignore_index=True)
    
    return sample_population

def create_timeseries(sample_population,start_date,end_date):
    col=['person_id','age_group_name','country','date','state','staying_days','prev_state']
    timeseries=pd.DataFrame(columns=col)
    date_range=pd.date_range(start=start_date,end=end_date)

    for _, row in sample_population.iterrows():
        distinct_timeseries=pd.DataFrame()
        distinct_timeseries = pd.DataFrame(index=range(len(date_range)))
        distinct_timeseries.loc[:,'person_id']=row['person_id']
        distinct_timeseries['age_group_name']=row['age_group_name']
        distinct_timeseries['country']=row['country']
        distinct_timeseries['date']=date_range
        state_list,prev_state_list,staying_days_list=markov_chain(len(date_range),row['age_group_name'],TRANSITION_PROBS,HOLDING_TIMES)
        distinct_timeseries['state']=state_list
        distinct_timeseries['staying_days']=staying_days_list
        distinct_timeseries['prev_state']=prev_state_list
        timeseries=pd.concat([timeseries,distinct_timeseries],ignore_index=True)
    return timeseries
    
def markov_chain(date_range,age_group,Transitions,holding_times):
    state_list,prev_state_list,staying_days_list=list(),list(),list()
    present, previous="H","H"
    count=0
    for days in range(date_range):
        state_list.append(present)
        staying_days_list.append(count)
        prev_state_list.append(previous)

        if count <=1:
            previous=present
            present=np.random.choice(list(Transitions[age_group][present].keys()),p=list(Transitions[age_group][present].values()))
            count=holding_times[age_group][present]
        else:
            previous=present
            count -=1                                     
    return state_list,prev_state_list,staying_days_list

def summarize_states(data):
    summary = data.groupby(['date', 'country', 'state']).size().unstack(fill_value=0)
    summary = summary.reindex(columns=['D', 'H', 'I', 'M', 'S'], fill_value=0).reset_index()

    return summary

def run(countries, sample_ratio, start_date, end_date):
    sample_population = sample_creation_corrected(sample_ratio, countries)
    sample_population.to_csv('sample.csv', index=False)
    
    timeseries=create_timeseries(sample_population,start_date,end_date)
    timeseries.to_csv('a2-covid-simulated-timeseries.csv',index=False)
    
    summarized_data = summarize_states(timeseries)
    summarized_data.to_csv('a2-covid-summary-timeseries.csv',index=False)

    helper.create_plot('a2-covid-summary-timeseries.csv',countries)