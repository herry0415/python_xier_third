# python_xier_third
西二在线第三轮考核
from flask import Flask,request,jsonify
import pymysql
from werkzeug.utils import redirect

conn = pymysql.connect(host='127.0.0.1', port=3306, user='root', password='Dranji04150092', charset='utf8',
                           db='day_1')
cursor = conn.cursor()
app = Flask(__name__)
@app.route("/todo/add_new",methods=["POST"])
def add_new():
    my_json = request.get_json()
    my_title = my_json.get("title")
    my_text = my_json.get("text")
    my_accomplish = my_json.get("accomplish")
    my_add_time = my_json.get("add_time")
    my_deadline = my_json.get("deadline")
    sql = 'insert into todolist (title,text,accomplish,add_time,deadline) values ("{}","{}","{}","{}")'.format(my_title,my_text,my_accomplish,my_add_time,my_deadline)
    cursor.execute(sql)
    conn.commit()
    return "添加成功"

@app.route("/todo/change/one/done",methods=["POST"])
def change_to_ready():
    my_json = request.get_json()
    idd = my_json.get("id")
    sql = 'update todolist set accomplish="YES" where id = idd'
    cursor.execute(sql)
    conn.commit()
    return "改变成功"

@app.route("/todo/change/all/done",methods=["POST"])
def change_to_ready():
    sql = 'update todolist set accomplish="YES" '
    cursor.execute(sql)
    conn.commit()
    return "right"

@app.route("/todo/change/one/undone",methods=["POST"])
def change_to_ready():
    my_json = request.get_json()
    idd = my_json.get("id")
    sql = 'update todolist set accomplish="NO" where id = {} ' % idd
    cursor.execute(sql)
    conn.commit()
    return "right"

@app.route("/todo/change/all/undone",methods=["POST"])
def change_to_ready():
    sql = 'update todolist set accomplish="NO" '
    cursor.execute(sql)
    conn.commit()
    return "right"

@app.route("/todo/check/one",methods=["POST"])
def check_one():
    my_json = request.get_json()
    idd = my_json.get("id")
    sql = 'select * from todolist where id = {}' % idd
    cursor.execute(sql)
    data = cursor.fetchall()
    print(data)
    return "right"

@app.route("/todo/check/all/done",methods=["POST"])
def check_all_done():
    sql = 'select * from todolist where accomplish = "YES" '
    cursor.execute(sql)
    data = cursor.fetchall()
    print(data)
    return "right"

@app.route("/todo/check/all/undone",methods=["POST"])
def check_all_undone():
    sql = 'select * from todolist where accomplish = "NO" '
    cursor.execute(sql)
    data = cursor.fetchall()
    print(data)
    return "find_all_undone"

@app.route("/todo/check/all/undone",methods=["POST"])
def check_all_undone():
    sql = 'select * from todolist '
    cursor.execute(sql)
    data = cursor.fetchall()
    print(data)
    return "find_all"

@app.route("/todo/delete/one",methods=["POST"])
def check_delete_one():
    my_json = request.get_json()
    idd = my_json.get("id")
    sql = 'delete from todolist  where id = {} ' % idd
    cursor.execute(sql)
    conn.commit()
    return "delete successful"

@app.route("/todo/delete/all",methods=["POST"])
def check_delete_all():
    sql = 'delete from todolist  where id > 0 '
    cursor.execute(sql)
    conn.commit()
    return "delete successful"
@app.route("/todo/first",methods=["POST"])
def first_post():
    try:
        my_json = request.get_json()
        getname = my_json.get("name")
        get_age = my_json.get("age")
        if not all([getname,get_age]):
            return jsonify(msg="缺少参数")
        print(my_json)
        return jsonify(name=getname,age=get_age)
    except Exception as e:
        print(e)
        return jsonify(msg="出错了哦，请查看是否正确")

@app.route("/heyfirst")
def todolist():
    return "hello todolist"

app.run(host="0.0.0.0")
