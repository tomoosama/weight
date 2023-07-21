# weight
input a weight
from flask import Flask, render_template, request, redirect, url_for
import sqlite3
from datetime import datetime

app = Flask(__name__)

# データベースの初期化
def init_db():
    conn = sqlite3.connect('weight_data.db')
    c = conn.cursor()
    c.execute('''CREATE TABLE IF NOT EXISTS weight
                 (id INTEGER PRIMARY KEY AUTOINCREMENT,
                 date TEXT NOT NULL,
                 weight REAL NOT NULL)''')
    conn.commit()
    conn.close()

# 体重データの保存
def save_weight(date, weight):
    conn = sqlite3.connect('weight_data.db')
    c = conn.cursor()
    c.execute('INSERT INTO weight (date, weight) VALUES (?, ?)', (date, weight))
    conn.commit()
    conn.close()

# 体重データの取得
def get_weights():
    conn = sqlite3.connect('weight_data.db')
    c = conn.cursor()
    c.execute('SELECT * FROM weight ORDER BY id DESC')
    weights = c.fetchall()
    conn.close()
    return weights

# ホームページの表示
@app.route('/')
def index():
    weights = get_weights()
    return render_template('index.html', weights=weights)

# 体重の入力処理
@app.route('/add_weight', methods=['POST'])
def add_weight():
    date = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    weight = float(request.form['weight'])
    save_weight(date, weight)
    return redirect(url_for('index'))

if __name__ == '__main__':
    init_db()
    app.run(debug=True)
