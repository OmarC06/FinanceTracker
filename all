from tkinter import *
from tkinter import messagebox
from tkcalendar import DateEntry
from tkinter.ttk import *
from csv import *
import os
from datetime import date
import numpy as np
import matplotlib.pyplot as plt

root = Tk()
root.title("Finance Tracker")
root.geometry("600x400")

with open("financeTracker.csv", "r", newline="") as file:
  r = reader(file)
  data = list(r)
  if not data[1:]:
    noData = True
  else: 
    noData = False
# this opens the file to be read
# if there is no data yet, it sets noData to be true

def raise_frame(frame):
  if noData and frame == budgeting_Stats:
    messagebox.showerror(title="Data Error", message="No data available. Please add data.")
  else:
    frame.tkraise()
  if frame == fullLog:
    resetFilter()
  # resets the table data when section is entered
  elif frame == budgeting_Stats:
    resetStats()
  # resets the stats when section is entered
# this is used to go to another section of the code.

financeLogger = Frame(root)
fullLog = Frame(root)
budgeting_Stats = Frame(root)
# these define the sections

for frame in (financeLogger, fullLog, budgeting_Stats):
  frame.grid(row=0, column=0, sticky='nsew')
# these stack them on top of each other

moneySpentL = Label(financeLogger, text="Money Spent: ").place(x=10, y=10)
moneySpentEnt = Entry(financeLogger)
moneySpentEnt.place(x=150, y=10)

itemL = Label(financeLogger, text="Item: ").place(x=10, y=30)
itemEnt = Entry(financeLogger)
itemEnt.place(x=150, y=30)

methodEnt2 = StringVar(financeLogger)
methodEnt2.set("Cash")

methodL = Label(financeLogger, text="Payment Method: ").place(x=10, y=50)
methodEnt = OptionMenu(financeLogger, methodEnt2, "Cash", "Cash", "Credit", "Debit")
methodEnt.place(x=150, y=50)

dateL = Label(financeLogger, text="Date: ").place(x=10, y=80)
dateEnt = DateEntry(financeLogger, selectmode='day')
dateEnt.place(x=150, y=80)
# these are the inputs and their labels

list2 = []
def add():
  try:
    money = float(moneySpentEnt.get())
    list1 = [money, itemEnt.get(), methodEnt2.get(), dateEnt.get_date()]
    list2.append(list1)
    messagebox.showinfo("Information", "Data to be logged. Make sure to save before exiting.")
  except:
    messagebox.showerror(title="Value Error", message="Insert valid data")
  moneySpentEnt.delete(0, END)
  itemEnt.delete(0, END)
# this function adds the data inputted to a list. it takes into account errors from inputting the wrong data type and returns an error. also empties the inputs after adding to the list 

def save():
  file_exists = os.path.isfile("financeTracker.csv")
  with open("financeTracker.csv", "a") as file:
    w = writer(file)
    if not file_exists:
      w.writerow(["Amount", "Item", "Payment Method", "Date"])
    w.writerows(list2)

  messagebox.showinfo("Information", "Data saved to file.")
  global noData
  noData = False
# this function saves the data to the file

addBtn = Button(financeLogger, text="Add to Log", command=add).place(x=150, y=100)
saveBtn = Button(financeLogger, text="Save Data", command=save).place(x=150, y=130)

logBtn = Button(financeLogger, text="See Full Log", command=lambda:raise_frame(fullLog)).place(x=150, y=160)
budgetBtn = Button(financeLogger, text="Set Budget", command=lambda:raise_frame(budgeting_Stats)).place(x=150, y=190)
# more buttons

raise_frame(financeLogger)
# this opens the first section

#######
months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December", "Full Year"] 

monthEnt2 = StringVar(fullLog)
monthEnt2.set("January")

monthL = Label(fullLog, text="Month: ").grid(row=1, column=0)
monthEnt = OptionMenu(fullLog, monthEnt2, months[0], *months)
monthEnt.grid(row=1, column=1)

yearL = Label(fullLog, text="Year: ").grid(row=2, column=0)
yearEnt = Entry(fullLog)
yearEnt.grid(row=2, column=1)
# inputs for the date filtering. also the list of months

def filterDate():
  for item in table.get_children():
    table.delete(item)
  # first it deletes all items in the list for the new ones to be added
  
  with open("financeTracker.csv", "r", newline="") as file, open("filteredData.csv", "w") as tempFile:
    w = writer(tempFile)
    r = reader(file)
  # opens the file to be read and makes a new file to store the filtered data
    
    if monthEnt2.get() == "Full Year" and yearEnt.get() != '':
      for row in r:
        if f"{yearEnt.get()}" == row[3][:4]:
          w.writerow(row)
          table.insert('', END, values=row)
    elif yearEnt.get() == '' and monthEnt2.get() != "Full Year":
      for row in r:
        monthNum = months.index(monthEnt2.get()) + 1
        if monthNum < 10:
          monthNum = '0' + str(monthNum)
        else:
          monthNum = str(monthNum)
        data2 = []
        for row in r:
          if f"{monthNum}" == row[3][5:7]:
            w.writerow(row)
            table.insert('', END, values=row)
    elif yearEnt.get() == '' and monthEnt2.get() == "Full Year":
      messagebox.showerror(title="Filter Error", message="Specify a filter in either input.")
      resetFilter()
    else:
      monthNum = months.index(monthEnt2.get()) + 1
      if monthNum < 10:
        monthNum = '0' + str(monthNum)
      else:
        monthNum = str(monthNum)
      data2 = []
      for row in r:
        if f"{yearEnt.get()}-{monthNum}" == row[3][:7]:
          w.writerow(row)
          table.insert('', END, values=row)
    # in this big if statement above in the function, it will see if the user puts in only a month, only a year, or both. for each option, it will loop through the data to see which data point matches the month, year, or both inputted and then insert those data points into the table

# this function above in general goes through the file to see which ones match the inputted dates and then put it in the table

def resetFilter():
  for item in table.get_children():
    table.delete(item)

  with open("financeTracker.csv", "r", newline="") as file:
    r = reader(file)
    data = list(r)
  
  if not noData:
    for x in data[1:]:
      table.insert('', END, values=x)
# delete all data from the table and then loops through the data to input all the data back into the table  

filterBtn = Button(fullLog, text="Filter Date", command=filterDate).grid(row=3, column=1)
resetBtn = Button(fullLog, text="Reset Filter", command=resetFilter).grid(row=4, column=1)
loggerBtn = Button(fullLog, text="Return to main page", command=lambda:raise_frame(financeLogger)).grid(row=6, column=1)

table = Treeview(fullLog, columns = data[0], show='headings')
table.grid(row=0, column=1)

table.heading(data[0][0], text='Amount')
table.column(data[0][0], minwidth=0, width=100, stretch=NO)
table.heading(data[0][1], text='Item')
table.column(data[0][1], minwidth=0, width=100, stretch=NO)
table.heading(data[0][2], text='Payment Method')
table.column(data[0][2], minwidth=0, width=100, stretch=NO)
table.heading(data[0][3], text='Date')
table.column(data[0][3], minwidth=0, width=100, stretch=NO)

scrollbar = Scrollbar(fullLog, orient=VERTICAL, command=table.yview)
table.configure(yscrollcommand=scrollbar.set)
scrollbar.grid(row=0, column=2, sticky='ns')
# these are the widgets for the buttons and then the table with a scrollbar

#######
global spent
spent = 0

with open("timeTrack.csv", 'r') as file:
  r = reader(file)
  budget = float(list(r)[0][2])
# gets the budget stored in the file

def budgetTime():
  try:
    budget = float(budgetEnt.get())    
    if spent > budget:
       SpendingL.config(foreground = "red")
    else:
       SpendingL.config(foreground = "black")
    # if you are over your budget the text changes color to red
    
    currentMonth = str(date.today())[5:7]  
    
    with open("timeTrack.csv", 'r') as file:
      r = reader(file)
      c = list(r)
      try:
        a = str(c[0][0])
        b = c[0][1]
      except:
        a = "notSet"
        b = None
      if a == "notSet" and b != currentMonth:
        checkingSomething = True
      elif a == "set":
        if b == currentMonth:
          checkingSomething = False
        else:
          checkingSomething = True
    
    if checkingSomething:
      with open("timeTrack.csv", 'w') as file:
        w = writer(file)
        w.writerow(["set", currentMonth, budget])
        
      currentBudgetL.config(text=f"Current Month's Budget: ${budget:.2f}")
    else:
      messagebox.showerror(title="Budget Error", message="Current Month's Budget Already Set. Cannot change until the beginning of next month.")
      
  except ValueError:
    messagebox.showerror(title="Value Error", message="Insert valid data") 
# this allows you to change the budget only when a new month starts if one is already sets. Otherwise it sets one.    

currentMonth = str(date.today())[5:7]

budgetL = Label(budgeting_Stats, text="Budget: ").place(x=10, y=10)
budgetEnt = Entry(budgeting_Stats)
budgetEnt.place(x=100, y=10)
budgetBtn = Button(budgeting_Stats, text="Set Budget", command=budgetTime).place(x=100, y=30)

currentBudgetL = Label(budgeting_Stats, text=f"Current Month's Budget: ${budget:.2f}")
currentBudgetL.place(x=10, y=60)
SpendingL = Label(budgeting_Stats, text="")
SpendingL.place(x=10, y=80)
SpendingAvgL = Label(budgeting_Stats, text="")
SpendingAvgL.place(x=10, y=100)

PaymentMethodUseL = Label(budgeting_Stats, text="")
PaymentMethodUseL.place(x=10, y=120)
# more labels for the stats and also an input for the budget

def resetStats():
  spent = 0
  with open("financeTracker.csv", "r", newline="") as file:
    r = reader(file)
    data = list(r)
  transactionsNum = 0
  if not noData:
    for x in data[1:]:
      if currentMonth in x[3]:
        spent += float(x[0])
        transactionsNum += 1
    if transactionsNum == 0:
      transactionsNum = 1
    avgSpent = spent/transactionsNum
  else:
    spent = 0
    transactionsNum = 0
    avgSpent = 0
  # if there is data, it will loop through the data and accumulate the total and then divide this by the number of data points to get the average spent

  SpendingL.config(text = f"Current Month's Total Spending: ${spent:.2f}")
  SpendingAvgL.config(text = f"Current Month's Average Spending: ${avgSpent:.2f} over {transactionsNum} transactions")
  # edits the text labels with updated stats

  paymentMethodNameTracker = {}
  paymentMethodNumberTracker = []
  for row in data[1:]:
    paymentMethodNumberTracker.append(row[2])
    paymentMethodNameTracker[row[2]] = paymentMethodNumberTracker.count(row[2])
  if not noData:
    mostUsed = max(paymentMethodNameTracker, key=paymentMethodNameTracker.get)
    leastUsed = min(paymentMethodNameTracker, key=paymentMethodNameTracker.get)
  else:
    mostUsed, leastUsed = "nothing", "nothing as well"
  PaymentMethodUseL.config(text=f'''Most used payment method is {mostUsed.lower()} at {paymentMethodNameTracker[mostUsed]} times 
and the least used is {leastUsed.lower()} at {paymentMethodNameTracker[leastUsed]}.''')
  # this section goes through the data and counts how many times each payment method is used. then using a max and min method, it edits the text label with the most and least used payment methods

# this function generally sets the stats on the page when the section is opened

MonthYears = []
if not noData:
  fullYearList = []
  for y in data[1:]:
    fullYearList.append(int(y[3][:4]))
  firstYear = min(fullYearList)
  latestYear = max(fullYearList)
  # puts all the years in the data to a list and finds the first and latest year with the min and max methods
  
  monthTotalFullList = {}

  for year in range(firstYear, latestYear + 1):
    #this is looping through each year in the data
    for month_num, month in enumerate(months[:-1], 1):
      # then in each loop it goes through each month
      if month_num < 10:
        month_num = '0' + str(month_num)
      else:
        month_num = str(month_num)    
      for z in data[1:]:
        if month_num == z[3][5:7] and year == int(z[3][:4]):
          if f'{month}-{year}' not in MonthYears:
            MonthYears.append(f'{month}-{year}')
        if f'{month}, {year}' not in monthTotalFullList:
          monthTotalFullList[f'{month}, {year}'] = 0
        if f'{year}-{month_num}-' in z[3]:
          monthTotalFullList[f'{month}, {year}'] += float(z[0])
        if monthTotalFullList[f'{month}, {year}'] == 0:
          del monthTotalFullList[f'{month}, {year}']
        # then in the above, it goes through each data point and if it matches the current month and year being looped through, that data point is added to a dictionary keyword with that month and year and the value being the total month spent in that month, year
  monthSpendings = list(monthTotalFullList.values())
else: 
  monthSpendings = []
  # if theres no data, theres no spendings
  
NumMonthsForEnt = [str(x) for x in range(2, len(MonthYears) + 1)]
NumEnt2 = StringVar(budgeting_Stats)
NumEnt2.set("2")

NumL = Label(budgeting_Stats, text="Number of Months Back: ").place(x=10, y=160)
NumEnt = OptionMenu(budgeting_Stats, NumEnt2, *NumMonthsForEnt)
NumEnt.place(x=170, y=160)

TrendL = Label(budgeting_Stats, text="")
TrendL.place(x=10, y=190)
# more widgets and an input for the number of months to look at the trend for

def seeTrends():
  try:
    with open("moneyTrends.csv", "w") as file:
      w = writer(file)
      for x in monthSpendings:
        w.writerow([x])
    # stores each months total spendings in a csv file
    
    with open("moneyTrends.csv", "r") as file:
      r = reader(file)
      data3 = list(r)
    # opens that same file to be read
    
    targetNumOfMonths = int(NumEnt2.get()) * -1
    targetSpentData = data3[targetNumOfMonths:]
    targetMonthSpendings = []
    for row in targetSpentData:
      targetMonthSpendings.append(float(row[0]))
    x = np.array(range(1, targetNumOfMonths * -1 + 1))
    y = np.array(targetMonthSpendings)
    # takes user input for the number of previous months to see trends for. then it adds the data for those months only into a list for the y axis of the table and the number of months to the x axis 
  
    plt.plot(x, y, 'o')
    #create basic scatterplot with the x and y
  
    m, b = np.polyfit(x, y, 1)
    #obtain m (slope) and b (intercept) of linear regression line
  
    if m > 0:
      TrendL.config(text = "You have been spending more, recently.")
    elif m < 0:
      TrendL.config(text = "You have been spending less, recently.")
    else:
      TrendL.config(text = "You have been spending the same, recently.")
    # if the slope is increasing, then money being spent is trending upward, if decreasing, then its trending down, if its a constant, then the same amount has been spent
    
    plt.plot(x, m*x+b)
    #add linear regression line to scatterplot 
    
    plt.show()
    # opens the graph
  except ValueError:
    messagebox.showerror(title="Data Error", message="No data found in previous months.")
    
TrendBtn = Button(budgeting_Stats, text = "See Trends", command=seeTrends).place(x=250,y=160)

loggerBtn2 = Button(budgeting_Stats, text="Return to main page", command=lambda:raise_frame(financeLogger)).place(x=10, y=200)
# last two buttons, one for the trends, the other to go back to the other page

root.mainloop()
# runs the whole tkinter window
