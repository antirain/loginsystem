import tkinter.messagebox
import os
import pymssql
import tkinter as tk

name = ''
num = ''
sign = ''
host = 'DESKTOP-SR60T52'
server = 'DESKTOP-SR60T52\SQLEXPRESS'
port = '49670'
user = 'sa'
password = '123'
database = 'login_system'


def stu_sign_in():
    def check():
        sql = 'select sign_in from student_list where name = %s'
        tuple = (name)
        with pymssql.connect(host=host,
                             server=server,
                             port=port,
                             user=user,
                             password=password,
                             database=database) as conn:
            with conn.cursor() as cursor:
                cursor.execute(sql, tuple)
                flag = cursor.fetchone()
                if flag == 1:
                    tkinter.messagebox.showinfo(title='提示', message='你已签到成功！')

    def sign_in():
        sql = 'update student_list set sign_in = 1 where name = %s'
        tuple = (name)
        try:
            with pymssql.connect(host=host,
                                 server=server,
                                 port=port,
                                 user=user,
                                 password=password,
                                 database=database) as conn:
                with conn.cursor() as cursor:
                    cursor.execute(sql, tuple)
                    conn.commit()
        except Exception:
            tkinter.messagebox.showerror(title='错误', message='姓名或工号错误！')
        else:
            tkinter.messagebox.showinfo(title='通知', message='写入成功！')

    sign = tk.Tk()
    sign.title('学生签到')
    btn = tk.Button(sign, text='签到', command=sign_in)
    btn2 = tk.Button(sign, text='查看记录', command=check)
    btn.pack()
    btn2.pack()
    sign.mainloop()


def tea_sign_in():
    def open():
        os.system('签到名单.txt')

    tea = tk.Tk()
    tea.title('教师功能')
    btn = tk.Button(tea, text='查看签到名单', command=open).pack()
    btn2 = tk.Button(tea, text='退出', command=tea.destroy).pack()
    tea.mainloop()


def stu_scr():
    def student_log_in():
        global name
        name = str(entry.get())
        global num
        num = str(entry2.get())
        sql = 'select name , num from student_list'
        try:
            with pymssql.connect(host=host,
                                 server=server,
                                 port=port,
                                 user=user,
                                 password=password,
                                 database=database) as conn:
                with conn.cursor() as cursor:
                    cursor.execute(sql)
                    row = cursor.fetchone()
                    while row:
                        if name == row[0]:
                            if num == row[1]:
                                stu_sign_in()
                        row = cursor.fetchone()
        except Exception:
            tkinter.messagebox.showerror(title='错误', message='姓名或学号错误！')
        else:
            tkinter.messagebox.showinfo(title='通知', message='写入成功！')

    stu = tk.Tk()
    stu.title('学生签到')
    label = tk.Label(stu, text='输入姓名')
    entry = tk.Entry(stu)
    label2 = tk.Label(stu, text='输入学号')
    entry2 = tk.Entry(stu)
    btn = tk.Button(stu, text='登录', command=student_log_in)
    label.pack()
    entry.pack()
    label2.pack()
    entry2.pack()
    btn.pack()
    stu.mainloop()


def tea_scr():
    def teacher_log_in():
        global name
        name = str(entry.get())
        global num
        num = str(entry2.get())
        sql = 'select * from teacher_list'
        try:
            with pymssql.connect(host=host,
                                 server=server,
                                 port=port,
                                 user=user,
                                 password=password,
                                 database=database) as conn:
                with conn.cursor() as cursor:
                    cursor.execute(sql)
                    row = cursor.fetchone()
                    while row:
                        if name == row[0]:
                            if num == row[1]:
                                tea_sign_in()
                        row = cursor.fetchone()
        except Exception:
            tkinter.messagebox.showerror(title='错误', message='姓名或工号错误！')
        else:
            tkinter.messagebox.showinfo(title='通知', message='写入成功！')

    tea = tk.Tk()
    tea.title('教师查询')
    label = tk.Label(tea, text='输入姓名')
    entry = tk.Entry(tea)
    label2 = tk.Label(tea, text='输入工号')
    entry2 = tk.Entry(tea)
    label.pack()
    entry.pack()
    label2.pack()
    entry2.pack()
    btn4 = tk.Button(tea, text='登录', command=teacher_log_in).pack()


def write_in_stu():
    def check():
        global name
        name = str(entry.get())
        global num
        num = str(entry2.get())
        sql = 'insert into student_list (name , num)values(%s,%s)'
        tuple = (name, num)
        try:
            with pymssql.connect(host=host,
                                 server=server,
                                 port=port,
                                 user=user,
                                 password=password,
                                 database=database) as conn:
                with conn.cursor() as cursor:
                    cursor.execute(sql, tuple)
                    conn.commit()
        except Exception:
            tkinter.messagebox.showerror(title='错误', message='已经写入过该学生信息！')
        else:
            tkinter.messagebox.showinfo(title='通知', message='写入成功！')

    stu = tk.Tk()
    stu.title('写入名单')
    stu.geometry('360x240')
    label1 = tk.Label(stu, text='输入姓名').pack()
    entry = tk.Entry(stu)
    entry.pack()
    label2 = tk.Label(stu, text='输入学号').pack()
    entry2 = tk.Entry(stu)
    entry2.pack()
    btn = tk.Button(stu, text='确认', command=check).place(x=100, y=100)
    btn2 = tk.Button(stu, text='退出', command=stu.destroy).place(x=220, y=100)
    stu.mainloop()


def write_in_tea():
    def check():
        global name
        name = str(entry.get())
        global num
        num = str(entry2.get())
        sql = 'insert into teacher_list (t_name , t_num)values(%s,%s)'
        tuple2 = (name, num)
        try:
            with pymssql.connect(host=host, server=server, port=port, user=user, password=password,
                                 database=database) as conn:
                with conn.cursor() as cursor:
                    cursor.execute(sql, tuple2)
                    conn.commit()
        except Exception:
            tkinter.messagebox.showerror(title='错误', message='已经写入过该教师信息！')
        else:
            tkinter.messagebox.showinfo(title='通知', message='写入成功！')

    tea = tk.Tk()
    tea.title('写入名单')
    tea.geometry('360x240')
    label1 = tk.Label(tea, text='输入姓名').pack()
    entry = tk.Entry(tea)
    entry.pack()
    label2 = tk.Label(tea, text='输入工号').pack()
    entry2 = tk.Entry(tea)
    entry2.pack()
    btn = tk.Button(tea, text='确认', command=check).place(x=100, y=100)
    btn2 = tk.Button(tea, text='退出', command=tea.destroy).place(x=220, y=100)
    tea.mainloop()


def main_frame():
    top = tk.Tk()
    top.title('签到系统')
    top.geometry('300x160')
    label = tk.Label(top, text='选择功能').place(x=125)
    btn = tk.Button(top, text='学生登录签到', command=stu_scr).place(x=55, y=40)
    btn2 = tk.Button(top, text='教师查看名单', command=tea_scr).place(x=175, y=40)
    btn3 = tk.Button(top, text='写入学生名单', command=write_in_stu).place(x=55, y=100)
    btn4 = tk.Button(top, text='写入教师名单', command=write_in_tea).place(x=175, y=100)
    top.mainloop()


if __name__ == '__main__':
    main_frame()
