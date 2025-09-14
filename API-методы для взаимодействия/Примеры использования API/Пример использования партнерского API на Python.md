# Пример использования партнерского API на Python

```python
import json
import requests
from datetime import date
 
 
def invoke(method: str, params: dict, auth_sid) -> dict:
    """Вызов метода"""
    payload = {
        "jsonrpc": "2.0",
        "method": method,
        "params": params,
        "protocol": 2,
        "id": 0
    }
    headers = {
        'Host': 'fix-reg.tensor.ru',
        'Content-Type': 'application/json-rpc; charset=utf-8',
        'Accept': 'application/json-rpc'
    }
 
    if auth_sid:
        headers['X-SBISSessionID'] = auth_sid
        url = 'https://fix-reg.tensor.ru/partner_api/service/'
    else:
        url = 'https://fix-reg.tensor.ru/auth/service/'
 
    r = requests.post(url, headers=headers, data=json.dumps(payload))
 
    return json.loads(r.text)
 
 
def login(user_name, pwd) -> str:
    """Авторизация"""
    return invoke('САП.Аутентифицировать', {"login": user_name, "password": pwd}, None)['result']
 
 
def get_history(sid, date_from: date, date_to: date, nav_key=None, point_sale=None, subpoints=False) -> dict:
    """Получение изменений"""
    params = {'DateFrom': date_from.strftime('%Y-%m-%d'),
              'DateTo': date_to.strftime('%Y-%m-%d'),
              'PointSale': point_sale,
              'SubPoints':subpoints,
              'NavigationKey': nav_key}
    return invoke('Billing.GetHistory', params, sid)['result']
 
 
if __name__ == '__main__':
    sid = login('petr-test', 'petr-test*12')
    res = get_history(sid, date.today(), date.today())
    for record in res['d']:
        print(record)