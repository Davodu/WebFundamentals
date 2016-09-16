project_path: /web/_project.yaml
book_path: /web/fundamentals/_book.yaml
description: При переходе к протоколу HTTPS на лице любого оператора сайта написаны одни и те же вопросы: Что тут вообще происходит? Что означают все эти слова о шифровании? В этом разделе мы коротко об этом расскажем

{# wf_review_required #}
{# wf_updated_on: 2015-03-26 #}
{# wf_published_on: 2015-03-27 #}

# Введение в терминологию обеспечения безопасности {: .page-title }

{% include "web/_shared/contributors/chrispalmer.html" %}
{% include "web/_shared/contributors/mattgaunt.html" %}



При переходе к протоколу HTTPS на лице любого оператора сайта написаны одни и те же вопросы: Что тут вообще происходит? Что означают все эти слова о шифровании? В этом разделе мы коротко об этом расскажем

### TL;DR {: .hide-from-toc }
- Открытые/закрытые ключи используются для шифрования и расшифровки сообщений между браузером и сервером
- Сертифицирующий орган – это организация, которая подтверждает соответствие открытых ключей общедоступным доменным именам в системе DNS (например, www.foobar.com)
- Запрос на получение сертификата (CSR) – это формат данных, который связывает открытый ключ с метаданными о владельце ключа



## Что такое пары открытых и закрытых ключей?

**Пара открытых/закрытых ключей** – это пара очень длинных чисел, которые можно использовать в качестве 
ключей шифрования и ключей расшифровки и между которыми существует конкретная математическая 
взаимосвязь. Распространенной системой для пар ключей является **[криптосистема
RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))**. **Открытый
ключ** используется для шифрования сообщений, которые затем могут быть 
расшифрованы с помощью соответствующего **закрытого ключа**. Ваш веб-сервер сообщит
свой открытый ключ всему миру, а клиенты (например, веб-браузеры), будут использовать его
для выделения безопасного канала вашему серверу.

## Что такое сертифицирующий орган?

**Сертифицирующий орган** – это организация, которая лицензирует 
присвоение публичных ключей публичным доменным именам (например, www.foobar.com).
Скажем, как клиент узнает, является ли конкретный открытый ключ _истинным_
открытым ключом для www.foobar.com? На самом деле никак. Сертифицирующий орган подтверждает, 
что конкретный ключ является истинным для конкретного сайта, с помощью
собственного закрытого ключа, чтобы **[криптографически подписать
](https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Подписи_сообщений)** открытый ключ
веб-сайта. Такую подпись невозможно подделать с помощью вычислительных мощностей.
У браузеров (и других клиентов) имеются **хранилища якорей доверия**, которые содержат
открытые ключи, принадлежащие известным сертифицирующим органам, а затем они используют открытые ключи 
**для криптографической проверки** подписей сертифицирующего органа.

**Сертификат X.509** – это формат данных, который связывает открытый ключ
с метаданными о лице, которому принадлежит этот ключ. В Интернете
владельцем ключа является оператор сайта, а важными метаданными является 
имя веб-сервера в системе DNS. Когда клиент соединяется с веб-сервисом по протоколу HTTPS, веб-сервер 
предъявляет клиенту свой сертификат для проверки. Клиент проверяет
, не истек ли срок годности сертификата, совпадает ли имя DNS с именем
сервера, к которому хочет подключиться клиент, а также якорь доверия сертифицирующего органа,
который подписал сертификат. В большинстве случаев сертифицирующие органы не подписывают напрямую
сертификаты веб-серверов; как правило, имеется **цепочка сертификатов**, которая ведет к
якорю доверия, к посредникам, обеспечивающим подпись, а затем к владельцу
сертификата самого веб-сервера (**конечное лицо**).

## Что такое запрос на получение сертификата?

**Запрос на получение сертификата (CSR)** – это формат данных, который, подобно 
сертификату, связывает открытый ключ с метаданными о лице,
, которое владеет этим ключом. Тем не менее, клиенты не взаимодействуют с CSR; этим занимаются сертифицирующие органы. Если вам нужна
сертификация открытого ключа вашего веб-сервера, вы посылаете сертифицирующему органу запрос на получение сертификата. Сертифицирующий орган
проверяет информацию в запросе на получение сертификата, а затем выпускает сертификат.
После этого он посылает вам окончательный сертификат
(скорее всего, цепочку сертификатов) и закрытый ключ, которые вы затем устанавливаете на своем веб-сервере.
