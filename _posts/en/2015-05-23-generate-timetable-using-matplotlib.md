---
layout: post
title: "Generate timetable using matplotlib"
lang: en
category: en
tags: [matplotlib, Python]
---
{% include JB/setup %}
{% include JB/langselect %}

<!--Generate timetable using matplotlib-->

##Introduction
The other day, I made timetables using matplotlib for a little issue.
It will be use for some conferences or symposiums.

##What does it do?
* Reading room name, beginning time (hour, minute), duration, and event names.
* Plot events with different colors with the room name.
* Plot event name and beginning time.

##Data format
You can make input data files like flowing one.
It contains room name, beginning time (hour), beginning time (minute), duration, and event name.
Make the room name as consecutive number.
In this input data,1 is 'Room A', 2 is 'Room B'.

{% highlight bash%}
1	9	0	90	A1
2	9	20	90	B1
3	9	40	90	C1
4	10	0	90	D1
5	10	20	90	E1
1	10	45	90	A2
2	11	5	90	B2
3	11	25	90	C2
4	11	45	90	D2
5	12	5	90	E2
1	12	30	50	A3
2	12	50	50	B3
3	13	10	50	C3
4	13	30	50	D3
5	13	50	50	E3
1	13	30	90	A4
2	14	0	60	B4
{% endhighlight %}
Save it as ``data1.txt``.

##Processing program
If you want to increase the number of room, you can add data to rooms and colors list.
If you want to increase the number of day, you can add data to input_files and day_labels list.

{% highlight python linenos%}
#!usr/bin/env python
#coding: utf-8
import matplotlib.pyplot as plt
rooms=['Room A','Room B', 'Room C', 'Room D', 'Room E']
colors=['pink', 'lightgreen', 'lightblue', 'wheat', 'salmon']

input_files=['data1.txt']
day_labels=['Day 1']


for input_file, day_label in zip(input_files, day_labels):
    fig=plt.figure(figsize=(10,5.89))
    for line in open(input_file, 'r'):
        data=line.split()
        event=data[-1]
        data=map(float, data[:-1])
        room=data[0]-0.48
        start=data[1]+data[2]/60
        end=start+data[3]/60
        # plot event
        plt.fill_between([room, room+0.96], [start, start], [end,end], color=colors[int(data[0]-1)], edgecolor='k', linewidth=0.5)
        # plot beginning time
        plt.text(room+0.02, start+0.05 ,'{0}:{1:0>2}'.format(int(data[1]),int(data[2])), va='top', fontsize=7)
        # plot event name
        plt.text(room+0.48, (start+end)*0.5, event, ha='center', va='center', fontsize=11)

    # Set Axis
    ax=fig.add_subplot(111)
    ax.yaxis.grid()
    ax.set_xlim(0.5,len(rooms)+0.5)
    ax.set_ylim(15.1, 8.9)
    ax.set_xticks(range(1,len(rooms)+1))
    ax.set_xticklabels(rooms)
    ax.set_ylabel('Time')

    # Set Second Axis
    ax2=ax.twiny().twinx()
    ax2.set_xlim(ax.get_xlim())
    ax2.set_ylim(ax.get_ylim())
    ax2.set_xticks(ax.get_xticks())
    ax2.set_xticklabels(rooms)
    ax2.set_ylabel('Time')


    plt.title(day_label,y=1.07)
    plt.savefig('{0}.png'.format(day_label), dpi=200)


{% endhighlight %}

##Output image
When you execute the programs, you can get the images below.

![outputimage]({{ site.url }}/assets/images/2015-05-23a.png)