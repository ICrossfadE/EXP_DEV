Щоб перевірити свій API ключ в postman потрібно:
- Вибрати метод `GET` (або інший метод, який потрібен).
- Ввести URL-адресу API, наприклад: `https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest`
-  Перейдіть на вкладку "Headers". 
	  - Додати новий заголовок `Key` з ім'ям `X-CMC_PRO_API_KEY` і значенням вашого API ключа.
	  - Додати  `Value` і ввести  API ключ.