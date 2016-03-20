# sizeyunsuan3

#writher Megau Bing And Surver Devin
#edit date 20160317

from fractions import Fraction#分数
from random import randint#随机数


def replace(line):
    line=line.replace('+',' + ')
    line=line.replace('-',' - ')
    line=line.replace('*',' * ')
    line=line.replace('/',' / ')
    line=line.replace('(',' ( ')
    line=line.replace(')',' ) ')
    line=line.replace('  ',' ')
    line=line.replace('=',' = ')
    return line

def calculate(operator_cal,operator_num1,operator_num2):
    answer=0
    if(operator_cal=="+"):
        answer=operator_num1+operator_num2
    if(operator_cal=="-"):
        answer=operator_num1-operator_num2
    if(operator_cal=="*"):
        answer=operator_num1*operator_num2
    if(operator_cal=="/"):
        answer=operator_num1/operator_num2
    #print"####结果",answer
    return answer


def result_get(str1):
    operator_anw=[""]*100#存取运算符的数组
    operator_ord=0#指针，计算运算符位置，统计运算符的个数

    figure_anw=[0]*100#存取运算数的数组
    figure_ord=0#指针，计算运算数位置，统计运算数的个数

    line=replace(str1)
    #print line
    line = line.split()
    for word in line:
        #print "word:",word
        if(word=="+"):
            operator_anw[operator_ord]="+"
            operator_ord=operator_ord+1
        elif(word=="-"):
            operator_anw[operator_ord]="-"
            operator_ord=operator_ord+1
        elif(word=="*"):
            operator_anw[operator_ord]="*"
            operator_ord=operator_ord+1
        elif(word=="/"):
            operator_anw[operator_ord]="/"
            operator_ord=operator_ord+1
        elif(word=="("):
            operator_anw[operator_ord]="("
            operator_ord=operator_ord+1
        elif(word==")"):
            operator_anw[operator_ord]=")"
            operator_ord=operator_ord+1
        elif(word=="="):
            if(operator_ord==2):#如果出现运算符剩两个的情况，运算第二个运算符
                figure_anw[1]=calculate(operator_anw[1],figure_anw[1],figure_anw[2])
            figure_anw[0]=calculate(operator_anw[0],figure_anw[0],figure_anw[1])
            #print figure_anw[0],"end"
            return figure_anw[0]

        else:
            word=int(word)
            word=Fraction(word,1)
            figure_anw[figure_ord]=word
            figure_ord=figure_ord+1
            #print "已存入数字",word
            #print "下一个数字位置",figure_ord
            #print "下一个运算符位置",operator_ord
            #print operator_anw
            #print figure_anw
        #判断并进行运算，进栈出栈

        #优先级进行判断，是否入栈是否运算（+—同一类，*/同一类）

        #*+问题
        if((word=="+"or word=="-")and operator_ord>1 and (operator_anw[operator_ord-1]=="*" or operator_anw[operator_ord-1]=="/")):
            figure_anw[figure_ord-2]=calculate(operator_anw[operator_ord-2],figure_anw[figure_ord-2],figure_anw[figure_ord-1])
            operator_anw[operator_ord-1]=""
            figure_anw[figure_ord-1]=0
            operator_ord=operator_ord-1
            operator_anw[operator_ord-1]=word
            figure_ord=figure_ord-1
            #print operator_anw
            #print figure_anw
            
            
        if(word==")"):#1判断是否出现右括号
            #运算函数
            #if 如果出现+*两层运算问题，这个if解决第一层*/
            if(operator_anw[operator_ord-3]=="+" or operator_anw[operator_ord-3]=="-"):
                figure_anw[figure_ord-2]=calculate(operator_anw[operator_ord-2],figure_anw[figure_ord-2],figure_anw[figure_ord-1])
                figure_anw[figure_ord-1]=0
                operator_anw[operator_ord-1]=""
                operator_anw[operator_ord-2]=")"
                figure_anw[figure_ord-1]=0
                operator_ord=operator_ord-1
                figure_ord=figure_ord-1
                
            #这段是将括号中残存的唯一运算符进行运算并消掉括号和运算符
            figure_anw[figure_ord-2]=calculate(operator_anw[operator_ord-2],figure_anw[figure_ord-2],figure_anw[figure_ord-1])
            figure_anw[figure_ord-1]=0
            operator_anw[operator_ord-3]=""
            operator_anw[operator_ord-2]=""
            operator_anw[operator_ord-1]=""
            operator_ord=operator_ord-3
            figure_ord=figure_ord-1

        #+*+问题 解决
        if((word=="+"or word=="-")and (operator_anw[operator_ord-2]=="*" or operator_anw[operator_ord-2]=="/")and operator_ord>1):
            figure_anw[figure_ord-2]=calculate(operator_anw[operator_ord-2],figure_anw[figure_ord-2],figure_anw[figure_ord-1])
            operator_anw[operator_ord-2]=word
            operator_anw[operator_ord-1]=""
            figure_anw[figure_ord-1]=0
            figure_ord=figure_ord-1
            operator_ord=operator_ord-1
            #print"*+"
                
        #++问题
        if((word=="+"or word=="-")and operator_ord>1):
            #print "************************************************",operator_anw[operator_ord-2]
            #print operator_anw[operator_ord-2]
            if(operator_anw[operator_ord-2]=="+"or operator_anw[operator_ord-2]=="-"):
                figure_anw[figure_ord-2]=calculate(operator_anw[operator_ord-2],figure_anw[figure_ord-2],figure_anw[figure_ord-1])
                operator_anw[operator_ord-1]=""
                figure_anw[figure_ord-1]=0
                operator_ord=operator_ord-1
                operator_anw[operator_ord-1]=word
                figure_ord=figure_ord-1
                #print operator_anw
                #print figure_anw


def layer(layer_accual2,operat_number2,brackets2,layer_amount2):#递归程序
    if(layer_accual2>0):#对第一层开始计算，将形成3个以上的数字，层数暂时为设定的3。
         #选择数字标号
        #print"layer_accual2",layer_accual2
        opreation_radom=randint(0,layer_accual2-1)#第一层加1，抽取号码，进行替换
        find_operat_number=operat_number[opreation_radom]
        #即两个数中选择一个数进行替换成为一个简单的四则二元运算
        #print "operater_num",operater_num
        #将选中的数字从第二层开始，用一个简单的二元运算式替换选中的数字，并插入数组
        #插入时依据数字编号判断是否加入括号，依据此数字所在的周围是否有*\符号
        #判断是否有添加括号
        if((operator[opreation_radom]=="-")or operator[opreation_radom+1]=="-")or operator[opreation_radom]=="/"or(operator[opreation_radom]=="*")or(operator[opreation_radom+1]=="/")or(operator[opreation_radom+1]=="*"):#判断选中数字周围的符号
            brackets[layer_accual2]=1
        if(multiplication_and_division==2):
            brackets[layer_accual2]=0


    operater_num=randint(1,multiplication_and_division)  #将运算符入数组
    operator_one="?"
    #operater_num=2
    if(operater_num==1):
        operator_one="+"
    if(operater_num==2):
        operator_one="-"
    if(operater_num==3):
        operator_one="*"
    if(operater_num==4):
        operator_one="/"
        
    if(layer_accual2==0):
        operator[1]=operator_one
    else:
    
        mov_amount=layer_accual2+2-opreation_radom
        for i in range(0,mov_amount):
            operator[layer_accual2+2-i]=operator[layer_accual2+2-i-1]
            #print"i",i 
        operator[opreation_radom+1]=operator_one
        
    zhen_zheng=randint(1,2)  #是真分数或者整数，随机
    if(fraction_exist==0):
        zhen_zheng=1
    if(zhen_zheng==1):          #产生第一个数字 
        first_num=randint(1,number_range)
        first_num=str(first_num)
    else:
        first_num1=2
        first_num2=1
        while (first_num1>=first_num2):
            first_num1=randint(1,number_range)
            first_num2=randint(1,number_range)
        first_num=Fraction(first_num1,first_num2)
        if(first_num!=0):
            first_num="("+str(first_num)+")"        
        first_num=str(first_num)
    zhen_zheng=randint(1,2)  #是真分数或者整数，随机
    if(fraction_exist==0):
        zhen_zheng=1
    if(zhen_zheng==1):          #产生第二个数字 
        second_num=randint(1,10)
        second_num=str(second_num)
    else:
        second_num1=2
        second_num2=1
        while (second_num1>=second_num2):
            second_num1=randint(1,number_range)
            second_num2=randint(1,number_range)
        second_num=Fraction(second_num1,second_num2)
        if(second_num!=0):
            second_num="("+str(second_num)+")"  

    if(layer_accual2==0):#第0层，将最开始的两个数字存入数组
        operat_number[0]=first_num
        operat_number[1]=second_num
        if(negative_exit==0):#(如果不存在负数)
            if(second_num>first_num and operator_one==2):
                while(second_num>=first_num):
                    second_num=randint(1,number_range)
                    
        if(remainder==0):#(如果不存在余数)
           if(operator_one==4):
                while(second_num%first_num!=0):
                    print"remainder"
                    second_num=randint(1,number_range)
                    

    #从第一层开始存入两个数字
    if(layer_accual2>0):
        mov_amount=layer_accual2+2-opreation_radom
        for i in range(0,mov_amount):
            operat_number[layer_accual2+1-i]=operat_number[layer_accual2+1-i-1]
        operat_number[opreation_radom]=first_num
        operat_number[opreation_radom+1]=second_num

    
    #整理算式
    if(layer_accual2==0):
        expressions=" "

    if(layer_accual2==1):
        tempperate1=str(operat_number[0])
        tempperate2=str(operat_number[1])
        expressions=operat_number[0]+operator[1]+operat_number[1]
      
    if(layer_accual2>1):
        #先找到替换数字，然后产生表达式2，用2替换表达式1
        #global expressions
        kk=str(operat_number[opreation_radom])
        expressions2=first_num+operator_one+second_num
        if ( brackets[layer_accual2]==1):
            expressions2="("+first_num+operator_one+second_num+")"

        
        #创建一个查找机制，寻找不同的数字将其替换？
        #while(same_amount>0):
        #print"上一层句子",expressions
        #print"替换句子",expressions2
        #print"用于替换的的数字",find_operat_number
        expressions=expressions.replace(find_operat_number," "+find_operat_number+" ")
        expressions3=""
        recording_1=0
        line=expressions.split()
        for word2 in line:
            if (word2==find_operat_number and recording_1==0):

                word2=expressions2
                recording_1=1
            expressions3=expressions3+word2
        expressions3=expressions3.replace(" ","")
        expressions=expressions3

    layer_accual2=layer_accual2+1
    if(layer_accual2<layer_amount2+1):
        layer(layer_accual2,operat_number2,brackets2,layer_amount2)


##############程序开始
expressions_amount=10#算式数量
layer_amount=3  #层数，即数的个数
number_range=20#整数数值的大小范围
fraction_exist=1#是否有分数
multiplication_and_division=4#是否有乘除，有则为4
negative_exit=0#负数是否存在，1存在
remainder=0#余数是否存在，1存在
pritenr=1#打印机模式
quit_num=1#退出的标志
#print "expressions_amount",expressions_amount




while(quit_num==1):
    
    right_amount=0
    print"打印方式，1为屏幕显示，2为导出为txt文件"
    temp=input()
    while(temp!=1 and temp!=2):
        print"输入错误，请重新输入"
        temp=input()
    pritenr=temp
    temp=1

    print"总数,至少大于等于1"
    temp=input()
    while(temp<1):
        print"输入错误，请重新输入"
        temp=input()
    expressions_amount=temp


    print"数值范围，（至少大于等于10）"
    temp=input()
    while(temp<10):
        print"输入错误，请重新输入"
        temp=input()
    number_range=temp

    print"参数个数,(大于1小于10)"
    temp=input()
    while(temp<2):
        print"输入错误，请重新输入"
        temp=input()
    layer_amount=temp-1
    
    print"是否有括号，支持十个参数参与计算,0为无括号，1为有括号"
    temp=input()
    while(temp!=1 and temp!=0):
        print"输入错误，请重新输入"
        temp=input()
    if(temp==1):
        multiplication_and_division=4
    if(temp==0):
        multiplication_and_division=2
        
    print"加减有无负数，0为无负数,1为有负数"
    temp=input()
    while(temp!=0 and temp!=1):
        print"输入错误，请重新输入"
        temp=input()
    negative_exit=temp

    print"加减有无分数，0为无分数,1为有分数"
    temp=input()
    while(temp!=0 and temp!=1):
        print"输入错误，请重新输入"
        temp=input()
    fraction_exist=temp
        
    print"除法有无余数，0为无余数，1为有余数"
    temp=input()
    while(temp!=1 and temp!=0):
        print"输入错误，请重新输入"
        temp=input()
    remainder=temp



    #答案记录
    answer_matrix=[0]*expressions_amount
    answer_matrix_human=[0]*expressions_amount


    
    #print expressions_mul
    print"请在等号后分别输入答案。形式为：分子，回车，分母，回车。如果答案为整数，分母请填1。"
    print"start"

    for counter1 in range(0,expressions_amount):
        #准备部分，执行参数，运算层数，运算到的层数
        layer_accual=0
        operator=['k']*(layer_amount+3)#记录运算符的记录
        operat_number=["?"]*(layer_amount+2)#记录运算数的记录器
        brackets=[0]*(layer_amount+1)#记录括号的存在标志
        operator[0]="?"
        operator[2]="?"
        layer(layer_accual,operat_number,brackets,layer_amount)
        expressions=expressions+"="
        #expressions_mul[counter1]=expressions#查重功能


        if(pritenr==1):#屏幕卷面显示
            print "第",counter1+1,"题",expressions,

            answer_matrix[counter1]=result_get(expressions)
            
            answer_user=raw_input()
            find_chu=0
            for i in answer_user:
                if(i=="/"):
                    find_chu=1
                    answer_user=answer_user.split("/")
                    answer_user_matrix_momentary=[0]*2
                    i=0
                    for answer_user_i in answer_user:
                        answer_user_matrix_momentary[i]=int(answer_user_i)
                        #print answer_user_i
                        i=i+1
                    answer_user=Fraction(answer_user_matrix_momentary[0],answer_user_matrix_momentary[1])
            if(find_chu==0):
                answer_user=Fraction(int(answer_user),1)
                
            
            answer_matrix_human[counter1]=answer_user#Fraction(fenzi,fenmu)
            if(answer_matrix[counter1]==answer_matrix_human[counter1]):
                print "√"
                right_amount=right_amount+1
                
            else:
                print "× 正确答案：",
                print answer_matrix[counter1]
            #result_get(expressions)
        else:
            f = file('output.txt', 'a')
            f.write(expressions+"\n")
            #f.write(expressions)

    print"题数：",expressions_amount
    print"答对的题数：",right_amount
    if(right_amount>0.7*expressions_amount):
        print"你太棒了"
    if(right_amount<0.6*expressions_amount):
        print"错的有点多，继续努力哦"
    print "参考答案："#,answer_matrix
    num=1
    for i in answer_matrix:
        print "第",num,"题：",
        num=num+1
        print i

    print"退出？0为退出，1为继续"
    temp=input()
    while(temp!=0 and temp!=1):
        print"请重新输入"
        temp=input()
    quit_num=temp

