# hello-world
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_excel('시도별 전출입 인구수.xlsx', encoding = 'cp949', fillna = 0, hader = 0)
df = df.fillna(method = 'ffill')

mask = (df['전출지별'] == '서울특별시') & (df['전입지별'] != '서울특별시')
df_seoul = df[mask]
df_seoul = df_seoul.drop('전출지별', axis = 1)
df_seoul = df_seoul.rename({'전입지별' : '전입지'}, axis = 1)
df_seoul = df_seoul.set_index('전입지')

sr_data = df_seoul.loc['경기도']

plt.rc('font', family = 'AppleGothic')

col_years = list(map(str, range(1970, 2018)))
df_3 = df_seoul.loc[['충청남도', '경상북도', '강원도'], col_years]

plt.style.use('ggplot')

fig = plt.figure(figsize = (20, 5))
ax = fig.add_subplot(1, 1, 1)

#그래프 안쪽 부분
ax.plot(df_3.loc['충청남도', :], marker = 'o', markerfacecolor = 'green', 
       markersize = 10, color = 'olive', linewidth = 2, label = '서울 > 충남')
ax.plot(df_3.loc['경상북도', :], marker = 'o', markerfacecolor = 'blue', 
       markersize = 10, color = 'skyblue', linewidth = 2, label = '서울 > 경북')
ax.plot(df_3.loc['강원도', :], marker = 'o', markerfacecolor = 'red', 
       markersize = 10, color = 'magenta', linewidth = 2, label = '서울 > 강원')

#그래프 바깥쪽 부분
ax.legend(loc = 'best')
ax.set_title('서울 > 충남, 경북, 강원 인구이동', size = 20)
ax.set_xlabel('기간', size = 12)
ax.set_ylabel('이동인구', size = 12)
ax.set_xticklabels(col_years, rotation = 70)
ax.tick_params(axis = 'x', labelsize = 10)
ax.tick_params(axis = 'y', labelsize = 10)

plt.show()



