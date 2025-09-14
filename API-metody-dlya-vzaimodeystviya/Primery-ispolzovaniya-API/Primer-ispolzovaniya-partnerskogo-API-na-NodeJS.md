# Пример использования партнерского API на NodeJS

```javascript
let http = require('http');

let login = "petr-test";
let password = "petr-test*12";

function invoke(url, method, params, sid) {
   let promise = new Promise(function (resolve, reject) {
      let postOptions = {
         method: "POST",
         headers: {
            "Content-Type": "application/json-rpc; charset=utf-8",
            "Accept": "application/json-rpc"
         }
      };

      let postData = JSON.stringify({
         "jsonrpc": "2.0",
         "method": method,
         "params": params,
         "protocol": 2,
         "id": 0
      });

      if (sid) {
         postOptions.headers["X-SBISSessionID"] = sid;
      }

      let postRequest = http.request(url, postOptions, function (res) {
         res.setEncoding("utf8");
         res.on("data", function (chunk) {
            let result = JSON.parse(chunk).result;
            resolve(result);
         });
      });
      postRequest.write(postData);
      postRequest.end();
   });

   return promise;
}

function getSID(login, password) {
   let promise = new Promise(function (resolve, reject) {
      let method = "СБИС.Аутентифицировать";
      let params = {
         "Логин": login,
         "Пароль": password
      };

      invoke('http://fix-reg.tensor.ru/auth/service/', method, params).then(function (result) {
         resolve(result);
      });
   });

   return promise;
}

getSID(login, password).then(function (sid) {
   let method = "Billing.GetHistory";
   let params = {
      "DateFrom": '2018-12-25',
      "DateTo": '2018-12-25',
      "PointSale": null,
      "SubPoints": false,
      "NavigationKey": null
   };
   invoke('http://fix-reg.tensor.ru/partner_api/service/', method, params, sid).then(function (result) {
      console.log(result);
   });
});