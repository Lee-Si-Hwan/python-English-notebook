# python-English-notebook
from tkinter import *
import easygui
import random
from openpyxl import load_workbook

#데이터를 불러옴
filename = "data.xlsx"
data = load_workbook(filename)
sheet = data.worksheets[0]
sheet_rows = sheet.max_row
sheet_cols = sheet.max_column

root = Tk()

#모드 변수
mn = ''

#처음에 모드 설정 창
def menu_mode():
    global mn
    mn = easygui.buttonbox("choice to want to mode", "menu", ("set word", 'word mode', 'meaning mode'))
    menu()
    

def menu():
    if mn == 'set word':
        title.config(text = mn)
        b1.config(text = 'save')
        ask.config(text = '')

    elif mn == 'word mode':
        title.config(text = mn)
        b1.config(text = 'next')
        setting()


    else:
        title.config(text = mn)
        setting()

        
        
        
        
def nextword():
    if mn == 'set word':
        print('save')
    if mn == 'word mode':
        print()
        
    
def All(event):
    if mn == 'word mode':
        wordQ()
    else:
        meaningQ()


def wordQ():
    if mn == 'set word':
        a()

    else:
        global lb1
        global l
        global x
        l = 2

        r = 0

        while True:
            if answer.get() == answers:
                lb1.config(text = '정답')
                break
    
            else:
                while sheet.cell(x,l).value != '.':
                    l += 1

                    if answer.get() == sheet.cell(x,l).value:
                        lb1.config(text = '정답')
                        r = 1
                        break
                    
                if r == 1:
                    print('bingo')

                else:
                    lb1.config(text = '틀림')
                    l = 2
                break
        
def meaningQ():
    global lb1
    global l
    global x

    if answer.get() == answers:
        lb1.config(text = '정답')
    else:
        lb1.config(text = '틀림')
    



#랜덤으로 엑셀파일에서 단어 불러옴

sheetlist = []
asks = ''
answers = ''
k = 0
l = 0
x = 0


def setting():
    for i in range(len(sheetlist)):
        del sheetlist[0]
        
    for i in range(1,sheet_rows+1):
        sheetlist.append(i)
    print('sh',sheetlist)
    rand()
    
def rand():
    global asks
    global answers
    global x
    means = []
    askword = ''

    if len(sheetlist) == 0:
        ask.config(text = '끝났습니다!')

    else:
        y = random.sample(sheetlist, 1)
        x = y[0]
        print(x)
        #한 번 출제된 문제 다신 안나오게 하기
        sheetlist.remove(x)
        
        if mn == 'word mode':
            k = 1
            l = 2
            
            asks = sheet.cell(x,k).value
            answers = sheet.cell(x,l).value
        else:
            l = 2
            while sheet.cell(x,l).value != '.':
                means.append(l)
                l += 1

            for i in range(2,len(means)+2):
                askword += sheet.cell(x,i).value

            asks = askword
            answers = sheet.cell(x,1).value



        ask.config(text = asks)


#단어 맞추는 메인 창 설정

title = Label(root, text = mn)

line = Label(root, text = '-------------------')
ask = Label(root, text = asks)


answer = Entry(root)
answer.bind("<Return>", All)


lb1 = Label(root, text = '')

b1 = Button(root, text = 'next', command = rand)

b2 = Button(root, text = 'back', command = menu_mode)

menu_mode()

title.pack()
line.pack()
ask.pack()
answer.pack()
lb1.pack()
b1.pack()
b2.pack()


root.mainloop()

#################################
#지금 단어랑 뜻 맞추기 진짜 레알 다했고 단어 추가하는 기능 해야됨.




