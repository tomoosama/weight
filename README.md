# weight
input a weight
<!DOCTYPE html>
<html>
<head>
    <title>バスケットボール選手体重管理アプリ</title>
</head>
<body>
    <h1>体重管理アプリ</h1>
    <form method="post" action="/add_weight">
        <label for="weight">体重（kg）：</label>
        <input type="number" step="0.1" name="weight" required>
        <input type="submit" value="追加">
    </form>
    <h2>体重履歴</h2>
    <table>
        <tr>
            <th>日時</th>
            <th>体重（kg）</th>
        </tr>
        {% for weight in weights %}
        <tr>
            <td>{{ weight[1] }}</td>
            <td>{{ weight[2] }}</td>
        </tr>
        {% endfor %}
    </table>
</body>
</html>
