---
title: Simple Plot Using Matplotlib
tags: [Python]
style: border
color: secondary
description: "Simple implementation of plot using Matplotlib"
---

Simple implementaion of ploting using matplot and pandas

```python
import pandas as pd
import matplotlib.pyplot as plt
import operator
import datetime
def plotting(requests,completed,wip,projects):
    column_label = ['Projects','Total Requests','Done','WIP','Not Started']
    plotting_fields = column_label[1:len(column_label)]
    not_started = map(operator.add,completed,wip)
    not_started = map(operator.sub,requests,not_started)
    raw_data = {column_label[0]:projects,column_label[1]: requests ,column_label[2]: completed,column_label[3]: wip,column_label[4]: not_started}
    df = pd.DataFrame(raw_data, columns = column_label)
    pos = list(range(len(df[column_label[1]])))
    width = 0.20
    fig, ax = plt.subplots()
    plt.style.use('seaborn')
    color_list =['#a170a8','#5ac1fc','#86ad80','#dbaca8']
    cell_text =[]
    for id,val in enumerate(plotting_fields):
        plt.bar([p + width * (id+1) for p in pos],df[val],width,alpha=0.5,color=color_list[id],label=df[column_label[0]][id])
        cell_text.append([x for x in df[column_label[id+1]]])
    ax.set_ylabel('No of Requests',color='Black',fontweight='bold',fontsize='medium')
    date = str(datetime.datetime.now()).split(' ')[0].format("%D-%m-%Y")
    ax.set_title('Request Queue\n {}'.format(date),color='Black',fontweight='bold',fontsize='large')
    ax.set_xticks([p + 1 * width for p in pos])
    ax.set_xticklabels(df[column_label[0]])
    plt.xlim(0,len(projects))
    plt.ylim([0, max(df[column_label[1]] + df[column_label[2]] + df[column_label[3]])])
    plt.legend(plotting_fields, loc='upper left')
    the_table = plt.table(cellText=cell_text,rowLabels=plotting_fields,
                          rowColours=color_list,colLabels=['','','',''],
                          loc='bottom',bbox=[0.0,-0.45,1,.45],cellLoc="center",rowLoc='left')
    plt.subplots_adjust(bottom=0.3,right=0.95,left=0.20)
    plt.savefig('Queue_Stats.png', dpi=450, facecolor='w', edgecolor='w',
            orientation='landscape', papertype=None, format=None,
            transparent=True, bbox_inches=None, pad_inches=0.1,
            frameon=None)
    plt.show()
if __name__ == "__main__":
    requests  = [5,6,7,11]
    completed =[2,4,6,7]
    wip =[1,2,1,3]
    projects = ['A','B','C','D']
    plotting(requests,completed,wip,projects)
```
