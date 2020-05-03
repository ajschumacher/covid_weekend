# Covid Weekend

```bash
git clone git@github.com:nytimes/covid-19-data.git
# at cf352f75dd5ae05b097c468f6837167b10e61d38

conda create -n covid_weekend python=3.7 statsmodels=0.11 jupyter matplotlib
conda activate covid_weekend
```

```python3
import csv

with open('covid-19-data/us.csv') as f:
    reader = csv.DictReader(f)
    lines = [line for line in reader]

for prev_day, day in zip(lines, lines[1:]):
    day['new_cases'] = int(day['cases']) - int(prev_day['cases'])
    day['new_deaths'] = int(day['deaths']) - int(prev_day['deaths'])

import datetime

for line in lines:
    line['day'] = datetime.datetime.strptime(line['date'],
                                             '%Y-%m-%d').strftime('%A')

with open('data.csv', 'w') as f:
    writer = csv.DictWriter(f, lines[1].keys())
    writer.writeheader()
    writer.writerows(lines[1:])
```

Then on to `analyze.ipynb`!
