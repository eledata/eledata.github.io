---
layout: post
keywords: Algorithms
description: Learn Algorithms - Dynamic programming
title: "Learn Algorithms Ten - Dynamic programming"
categories: Algorithms C++&Python
tags: Algorithms
group: archive
---

10.1 Dynamic programming

What is the Dynamic programming?

Dynamic programming, like the divide-and-conquer method, solves problems by combining the solutions to subproblems.

A Dynamic programming algorithm solves every subsubproblem just once and then saves its anser in a table, thereby avoiding the work of recomputing the answer every tiem the subsubproblem is encountered.

There are four steps:

  1. Characterize the structure of an optimal solution
  
  2. Recursively define the value of an optimal solution
  
  3. Compute the value of an optimal solution in a bottom-up fashion
  
  4. Construct an optimal solution from computed information
  
10.1.1 Assembly-Line scheduling

There four steps to assolve this problem:

Step 1: The structure of the fastest way through the factory.

Step 2: A recursive solution.

Step 3: Computing the fastest times.

Step 4: Constructing the fastest way through the factory.

Assembly_Line_Scheduling Code:
```
    #include <iostream>
    
    using namespace std;
    
    void dynamic_assline_schedule()
    {
      int i,j;
    	int temp;
    	int schedule_rec_one[10] = {0};
    	int schedule_rec_two[10] = {0};
    	int tot_time[2][10];
    	int ass_station_time_assumption[2][10];
    	int ass_sta_tranfer_time_assumption[2][10];
    
    	temp = 0;
    	memset(tot_time,0,sizeof(tot_time));
    	memset(ass_station_time_assumption,0,sizeof(ass_station_time_assumption));
    	memset(ass_sta_tranfer_time_assumption,0,sizeof(ass_sta_tranfer_time_assumption));
    
    	cout << "ass_station_time_assumption:" << endl;
    /*	for(i = 1; i <= 2; i++)
    	{
    		if (i == 1)
    		{
    			cout << "Station One:" << endl;
    		}
    		else
    		{
    			cout << "Station Two:" << endl;
    		}
    
    		for(j = 1; j <= 6; j++)
    		{
    			cin >> ass_station_time_assumption[i][j];
    		}
    
    	}
    
    	cout << "ass_sta_tranfer_time_assumption:" << endl;
    
    	for(i = 1; i <= 2; i++)
    	{
    		if (i == 1)
    		{
    			cout << "Transfer One:" << endl;
    		}
    		else
    		{
    			cout << "Transfer Two:" << endl;
    		}
    
    		for(j = 1; j <= 7; j++)
    		{
    			cin >> ass_sta_tranfer_time_assumption[i][j];
    		}
    	}*/
    	ass_station_time_assumption[1][1] = 7;
    	ass_station_time_assumption[1][2] = 9;
    	ass_station_time_assumption[1][3] = 3;
    	ass_station_time_assumption[1][4] = 4;
    	ass_station_time_assumption[1][5] = 8;
    	ass_station_time_assumption[1][6] = 4;
    
    	ass_station_time_assumption[2][1] = 8;
    	ass_station_time_assumption[2][2] = 5;
    	ass_station_time_assumption[2][3] = 6;
    	ass_station_time_assumption[2][4] = 4;
    	ass_station_time_assumption[2][5] = 5;
    	ass_station_time_assumption[2][6] = 7;
    
    	ass_sta_tranfer_time_assumption[1][1] = 2;
    	ass_sta_tranfer_time_assumption[1][2] = 2;
    	ass_sta_tranfer_time_assumption[1][3] = 3;
    	ass_sta_tranfer_time_assumption[1][4] = 1;
    	ass_sta_tranfer_time_assumption[1][5] = 3;
    	ass_sta_tranfer_time_assumption[1][6] = 4;
    	ass_sta_tranfer_time_assumption[1][7] = 3;
    
    	ass_sta_tranfer_time_assumption[2][1] = 4;
    	ass_sta_tranfer_time_assumption[2][2] = 2;
    	ass_sta_tranfer_time_assumption[2][3] = 1;
    	ass_sta_tranfer_time_assumption[2][4] = 2;
    	ass_sta_tranfer_time_assumption[2][5] = 2;
    	ass_sta_tranfer_time_assumption[2][6] = 1;
    	ass_sta_tranfer_time_assumption[2][7] = 2;
    
    	schedule_rec_one[1] = 1;
    	schedule_rec_two[1] = 2;
    
    	tot_time[1][1] = ass_station_time_assumption[1][1] + ass_sta_tranfer_time_assumption[1][1]; //9
    	tot_time[2][1] = ass_station_time_assumption[2][1] + ass_sta_tranfer_time_assumption[2][1]; //12
    
    	for(i = 2; i <= 6; i++)
    	{
    		if ((tot_time[1][i - 1] + ass_station_time_assumption[1][i]) < (tot_time[2][i - 1] + ass_sta_tranfer_time_assumption[2][i] + ass_station_time_assumption[1][i]))
    		{
    			schedule_rec_one[i] = 1;
    			temp = (tot_time[1][i - 1] + ass_station_time_assumption[1][i]);
    			tot_time[1][i] = temp;
    		}
    		else
    		{
    			schedule_rec_one[i] = 2;
    			temp = (tot_time[2][i - 1] + ass_sta_tranfer_time_assumption[2][i] + ass_station_time_assumption[1][i]);
    			tot_time[1][i] = temp;
    		}
    
    		if ((tot_time[2][i - 1] + ass_station_time_assumption[2][i]) < (tot_time[1][i - 1] + ass_sta_tranfer_time_assumption[1][i] + ass_station_time_assumption[2][i]))
    		{
    			schedule_rec_two[i] = 2;
    			temp = (tot_time[2][i - 1] + ass_station_time_assumption[2][i]);
    			tot_time[2][i] = temp;
    		}
    		else
    		{
    			schedule_rec_two[i] = 1;
    			temp = (tot_time[1][i - 1] + ass_sta_tranfer_time_assumption[1][i] + ass_station_time_assumption[2][i]);
    			tot_time[2][i] = temp;
    		}
    	}
    
    	if (tot_time[1][6] < tot_time[2][6])
    	{
    		tot_time[1][7] = tot_time[1][6] + ass_sta_tranfer_time_assumption[1][7];
    		schedule_rec_one[7] = 1;
    		cout << "Schedule one use time:"<< tot_time[1][7] << endl;
    		cout << "Trace:" << endl;
    		for(i = 1; i <= 7; i++)
    		{
    			cout << schedule_rec_one[i] << "_";
    		}
    	}
    	else
    	{
    		tot_time[2][7] = tot_time[2][6] + ass_sta_tranfer_time_assumption[2][7];
    		schedule_rec_two[7] = 2;
    		cout << "Schedule two use time:"<< tot_time[1][7] << endl;
    		cout << "Trace:" << endl;
    		for(i = 1; i <= 7; i++)
    		{
    			cout << schedule_rec_two[i] << "_";
    		}
    	}
    }
    
    int main()
    {
    	cout<<"Dynamic Schedule:" << endl;
    	dynamic_assline_schedule();
    	return 0;
    }
```   
    
10.1.2 Matrix-chain multiplication



