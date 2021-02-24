## TCP
TCP (Transmission Control Protocol) – это сетевой протокол, который «заточен» под соединение. Иными словами, прежде, чем начать обмен данными, данному протоколу требуется установить соединение между двумя хостами. Данный протокол имеет высокую надежность, поскольку позволяет не терять данные при передаче, запрашивает подтверждения о получении от принимающей стороны и в случае необходимости отправляет данные повторно. При этом отправляемые пакеты данных сохраняют порядок отправки, то есть можно сказать, что передача данных упорядочена. Минусом данного протокола является относительно низкая скорость передачи данных, за счет того что выполнение надежной и упорядоченной передачи занимает больше времени, чем в альтернативном протоколе UDP.
- **Инициализация соединения:**

Установка соединения осуществляется с помощью, так называемого трехстороннего рукопожатия TCP

- SYN — Клиент генерирует случайное число, например, x, и отправляет его на сервер.
- SYN ACK — Сервер подтверждает этот запрос, посылая обратно пакет ACK, состоящий из случайного числа, выбранного сервером (допустим, y), и числа x + 1, где x — число, пришедшее от клиента.
- ACK — клиент увеличивает число y, полученное от сервера и посылает y + 1 на сервер.

- **Загрузка данных:**

После инициализации соединения полезная нагрузка будет перемещаться в обоих направлениях TCP-соединения. Все пакеты в обязательном порядке будут содержать установленный флаг ACK. Другие флаги, такие как, например, PSH или URG, могут быть, а могут и не быть установленными.

- **Завершение соединения:**

При нормальном завершении TCP-соединения в большинстве случаев инициализируется процедура, называемая двухсторонним рукопожатием, в ходе которой каждая сторона закрывает свой конец виртуального канала и освобождает все задействованные ресурсы. Обычно эта фаза начинается с того, что один из задействованных процессов приложения сигнализирует своему уровню TCP, что сеанс связи больше не нужен.

- **Keep-alive или повторное использование соединений:**

На уровне TCP нет сообщений типа «keep-alive», и поэтому, даже если сеанс соединения в какой-то момент времени становится неактивным, он все равно будет продолжаться до тех пор, пока не будет отправлен следующий пакет.

Когда мы отправляем HTTP-запрос по сети, нам сразу нужно создать TCP-соединение. Однако в HTTP 1.0 возможность повторного использования соединения по умолчанию закрыта (если заголовок «keep-alive = close» дополнительно не включен в заголовок HTTP), то есть TCP-соединение автоматически закрывается после получения запроса и отправки ответа. Так как процесс создания TCP-соединения относительно затратный (он требует дополнительных затрат процессорных ресурсов и памяти, а также увеличивает сетевой обмен между сервером и клиентом, что особенно становится актуальным при создании защищенных соединений), то все это увеличивает количество лагов и повышает вероятность перегрузки сети. Поэтому для HTTP 1.1 было решено оставлять TCP-соединение открытым до тех пор, пока одна из сторон не решит прекратить его.

С другой стороны, если соединения не будут закрываться после того, как клиенты получат все необходимые им данные, задействованные ресурсы сервера для поддержания этих соединений не будут доступны другим клиентам. Поэтому HTTP-серверы, чтобы обеспечить больший контроль над потоком данных, используют временные интервалы (таймауты) для поддержки функциональности «keep-alive» для неактивных соединений (длящихся по умолчанию, в зависимости от архитектуры и конфигурации сервера, не более нескольких десятков секунд, а то и просто нескольких секунд), а также максимальное число отправляемых запросов «keep-alive», прежде чем сеанс без активного соединения будет остановлен. Более подробно о функциональности «keep-alive» вы можете узнать здесь

## UDP
UDP (User Datagram Protocol), в свою очередь, более прост. Для передачи данных ему не обязательно устанавливать соединение между отправителем и получателем. Информация передается без предварительной проверки готовности принимающей стороны. Это делает протокол менее надежным – при передаче некоторые фрагменты данных могут теряться. Кроме того, упорядоченность данных не соблюдается – возможен непоследовательный прием данных получателем. Зато скорость передачи данных по данному транспортному протоколу будет более высокой.
## TCP VS UDP
- **Надежность:** в этом случае предпочтительнее будет протокол TCP, за счет подтверждения получения данных, повторной отправки в случае необходимости, а также использованию такого инструмента как тайм-аут. Протокол UDP такого инструментария не имеет, а потому при получении отправленные данные могут приходить не полностью;
- **Упорядоченность:** опять будет предпочтительнее TCP, поскольку этот протокол гарантирует передачу пакетов данных именно в том порядке, в котором они были отправлены. В случае с UDP такой порядок не соблюдается;
- **Скорость**: здесь уже лидировать будет UDP, так как более тяжеловесному TCP-протоколу будет требоваться больше времени для установки соединения, подтверждения получения, повторной отправки данных и т.д. ;
- **Метод передачи данных:** в случае с TCP данные передаются потоково, границы фрагментов данных не имеют обозначения. В случае с UDP данные передаются в виде датаграмм – проверка пакетов на целостность осуществляется принимающей стороной только в случае получения сообщения. Также пакеты данных имеют определенные обозначения границ;

TCP применяется там, где требуется точная и подтверждаемая передача данных – например, отправка фотографий, или переписка между пользователями. UDP, в свою очередь, нужен для общения в голосовом формате, или при передаче потокового видео, например, с веб-камер или IP-камер.

**Однонаправленное (unicast) сообщение** отправляется из одного узла только в один другой узел. Это также называется связью "точка-точка". Протокол TCP поддерживает лишь однонаправленную связь. Если серверу нужно с помощью TCP взаимодействовать с несколькими клиентами, каждый клиент должен установить соединение, поскольку сообщения могут отправляться только одиночным узлам.

**Широковещательная передача (broadcast)** означает, что сообщение отправляется всем узлам сети. Групповая рассылка (multicast) - это промежуточный механизм: сообщения отправляются выбранным группам узлов.

UDP может использоваться для однонаправленной связи, если требуется быстрая передача, например для доставки мультимедийных данных, но главные преимущества UDP касаются широковещательной передачи и групповой рассылки.

Обычно, когда мы отправляем широковещательные или групповые сообщения, не нужно получать подтверждения из каждого узла, поскольку тогда сервер будет наводнен подтверждениями, а загрузка сети возрастет слишком сильно. Примером широковещательной передачи является служба времени. Сервер времени отправляет широковещательное сообщение, содержащее текущее время, и любой хост, если пожелает, может синхронизировать свое время с временем из широковещательного сообщения.

## TLS

Протокол TLS (transport layer security) основан на протоколе SSL (Secure Sockets Layer), изначально разработанном в Netscape для повышения безопасности электронной коммерции в Интернете. Протокол SSL был реализован на application-уровне, непосредственно над TCP (Transmission Control Protocol), что позволяет более высокоуровневым протоколам (таким как HTTP или протоколу электронной почты) работать без изменений.

После того, как протокол SSL был стандартизирован IETF (Internet Engineering Task Force), он был переименован в TLS. Поэтому хотя имена SSL и TLS взаимозаменяемы, они всё-таки отличаются, так как каждое описывает другую версию протокола.

Протокол TLS предназначен для предоставления трёх услуг всем приложениям, работающим над ним, а именно: шифрование, аутентификацию и целостность. Технически, не все три могут использоваться, однако на практике, для обеспечения безопасности, как правило используются все три:

- Шифрование – сокрытие информации, передаваемой от одного компьютера к другому:

Для того чтобы установить криптографически безопасный канал данных, узлы соединения должны согласовать используемые методы шифрования и ключи. Протокол TLS однозначно определяет данную процедуру, подробнее это рассмотрено в пункте TLS Handshake. Следует отметить, что TLS использует криптографию с открытым ключом, которая позволяет узлам установить общий секретный ключ шифрования без каких-либо предварительных знаний друг о друге.

- Аутентификация – проверка авторства передаваемой информации:

В рамках процедуры TLS Handshake имеется возможность установить подлинность личности и клиента, и сервера. Например, клиент может быть уверен, что сервер, которые предоставляет ему информацию о банковском счёте, действительно банковский сервер. И наоборот: сервер компании может быть уверен, что клиент, подключившийся к нему – именно сотрудник компании, а не стороннее лицо (данный механизм называется Chain of Trust)

- Целостность – обнаружение подмены информации подделкой.

TLS обеспечивает отправку каждого сообщения с кодом MAC (Message Authentication Code), алгоритм создания которого – односторонняя криптографическая функция хеширования (фактически – контрольная сумма), ключи которой известны обоим участникам связи. Всякий раз при отправке сообщения, генерируется его MAC-значение, которое может сгенерировать и приёмник, это обеспечивает целостность информации и защиту от её подмены.

**TLS Handshake:**

- Hello - The handshake begins with the client sending a ClientHello message. This contains all the information the server needs in order to connect to the client via SSL, including the various cipher suites and maximum SSL version that it supports. The server responds with a ServerHello, which contains similar information required by the client, including a decision based on the client’s preferences about which cipher suite and version of SSL will be used.
- Certificate Exchange - Now that contact has been established, the server has to prove its identity to the client. This is achieved using its SSL certificate, which is a very tiny bit like its passport. An SSL certificate contains various pieces of data, including the name of the owner, the property (eg. domain) it is attached to, the certificate’s public key, the digital signature and information about the certificate’s validity dates. The client checks that it either implicitly trusts the certificate, or that it is verified and trusted by one of several Certificate Authorities (CAs) that it also implicitly trusts. Much more about this shortly. Note that the server is also allowed to require a certificate to prove the client’s identity, but this typically only happens in very sensitive applications.
- Key Exchange - The encryption of the actual message data exchanged by the client and server will be done using a symmetric algorithm, the exact nature of which was already agreed during the Hello phase. A symmetric algorithm uses a single key for both encryption and decryption, in contrast to asymmetric algorithms that require a public/private key pair. Both parties need to agree on this single, symmetric key, a process that is accomplished securely using asymmetric encryption and the server’s public/private keys.

**Обмен ключами в протоколе TLS:**

The client generates a random key to be used for the main, symmetric algorithm. It encrypts it using an algorithm also agreed upon during the Hello phase, and the server’s public key (found on its SSL certificate). It sends this encrypted key to the server, where it is decrypted using the server’s private key, and the interesting parts of the handshake are complete. The parties are sufficiently happy that they are talking to the right person, and have secretly agreed on a key to symmetrically encrypt the data that they are about to send each other. HTTP requests and responses can now be sent by forming a plaintext message and then encrypting and sending it. The other party is the only one who knows how to decrypt this message, and so Man In The Middle Attackers are unable to read or modify any requests that they may intercept.

Ясно, что установление соединения TLS является, вообще говоря, длительным и трудоёмким процессом, поэтому в стандарте TLS есть несколько оптимизаций. В частности, имеется процедура под названием “abbreviated handshake”, которая позволяет использовать ранее согласованные параметры для восстановления соединения (естественно, если клиент и сервер устанавливали TLS-соединение в прошлом).

Также имеется дополнительное расширение процедуры Handshake, которое имеет название TLS False Start. Это расширение позволяет клиенту и серверу начать обмен зашифрованными данными сразу после установления метода шифрования, что сокращает установление соединения на одну итерацию сообщений.

На текущий момент, все браузеры при установке соединения TLS отдают предпочтение именно сочетанию алгоритма Диффи-Хеллмана и использованию временных ключей для повышения безопасности соединения.

Следует ещё раз отметить, что шифрование с открытым ключом используется только в процедуре TLS Handshake во время первоначальной настройки соединения. После настройки туннеля в дело вступает симметричная криптография, и общение в пределах текущей сессии зашифровано именно установленными симметричными ключами. Это необходимо для увеличения быстродействия, так как криптография с открытым ключом требует значительно больше вычислительной мощности.

## HTTP
Соединение управляется на транспортном уровне, и потому принципиально выходит за границы HTTP. Хотя HTTP не требует, чтобы базовый транспортного протокол был основан на соединениях,  требуя только надёжность, или отсутствие потерянных сообщений (т.е. как минимум представление ошибки). Среди двух наиболее распространенных транспортных протоколов Интернета, TCP надёжен, а UDP -- нет. HTTP впоследствии полагается на стандарт TCP, являющийся основанным на соединениях, несмотря на то, что соединение не всегда требуется.

HTTP/1.0 открывал TCP-соединение для каждого обмена запросом/ответом, имея два важных недостатка: открытие соединения требует нескольких обменов сообщениями, и потому медленно, хотя становится более эффективным при отправке нескольких сообщений, или при регулярной отправке сообщений: теплые соединения более эффективны, чем холодные.

Для смягчения этих недостатков, HTTP/1.1 предоставил конвеерную обработку (которую оказалось трудно реализовать) и устойчивые соединения: лежащее в основе TCP соединение можно частично контролировать через заголовок  Connection. HTTP/2 сделал следующий шаг, добавив мультиплексирование сообщений через простое соединение, помогающее держать соединение теплым и более эффективным.

Общие функции, управляемые с HTTP:

- Кэш: Сервер может инструктировать прокси и клиенты: что и как долго кэшировать. Клиент может инструктировать прокси промежуточных кэшей игнорировать хранимые документы.

- Ослабление ограничений источника: Для предотвращения шпионских и других, нарушающих приватность, вторжений, веб-браузер обчеспечивает строгое разделение между веб-сайтами. Только страницы из того же источника могут получить доступ к информации на веб-странице. Хотя такие ограничение нагружают сервер, заголовки HTTP могут ослабить строгое разделение на стороне сервера, позволяя документу стать частью информации с различных доменов (по причинам безопасности).

- Аутентификация: Некоторые страницы доступны только специальным пользователям. Базовая аутентификация может предоставляться через HTTP, либо через использование заголовка WWW-Authenticate и подобных ему, либо с помощью настройки спецсессии, используя куки.

- Прокси и тунелирование: Серверы и/или клиенты часто располагаются в интернете, и скрывают свои истинные IP-адреса от других. HTTP запросы идут через прокси для пересечения этого сетевого барьера. Не все прокси -- HTTP прокси. SOCKS-протокол, например, оперирует на более низком уровне. Другие, как, например, ftp, могут быть обработаны этими прокси.

- Сессии: Использование HTTP кук позволяет связать запрос с состоянием на сервере. Это создает сессию,  хотя ядро HTTP -- протокол без состояния.  Это полезно не только для корзин в интернет-магазинах, но также для любых сайтов, позволяющих пользователю настроить выход.

Когда клиент хочет взаимодействовать с сервером, являясь конечным сервером или промежуточным прокси, он выполняет следующие шаги:

- Открытие TCP соединения: TCP-соедиенение будет использоваться для отправки запроса или запросов, и получения ответа. Клиент может открыть новое соединение, переиспользовать существующее, или открыть несколько TCP-соединений к серверу.

- Отправка HTTP-сообщения: HTTP-собщения (до HTTP/2) -- человеко-читаемо. Начиная с HTTP/2, простые сообщения инкапсилуруются во фреймы, делая невозможным их чтения напрямую, но принципиально остаются такими же

- Читает ответ от сервера

- Закрывает или переиспользует соединение для дальнейших запросов

Если активирован HTTP-конвеер, несколько запросов могут быть отправлены без ожидания получения первого ответа целиком. HTTP-конвеер тяжело внедряется в существующие сети, где старые куски ПО сосуществуют с современными версиями.  HTTP-конвеер был заменен в HTTP/2 на более надежные мультиплексивные запросы во фрейме.

Запросы содержат следующие элементы:

- HTTP-метод
- Путь к ресурсу
- Версию HTTP-протокола
- HTTP Заголовки
- Опционально тело

Ответы содержат следующие элементы:

- Версию HTTP-протокола
- HTTP код состояния
- Сообщение состояния
- HTTP Заголовки
- Опционально тело

## HTTP/1.1

- Новые HTTP-методы — PUT, PATCH, HEAD, OPTIONS, DELETE.
- Постоянные соединения. Как говорилось выше, в HTTP/1.0 одно соединение обрабатывало лишь один запрос и после этого сразу закрывалось, что вызывало серьёзные проблемы с производительностью и проблемы с задержками. В HTTP/1.1 появились постоянные соединения, т.е. соединения, которые по умолчанию не закрывались, оставаясь открытыми для нескольких последовательных запросов. Чтобы закрыть соединение, нужно было при запросе добавить заголовок Connection: close. Клиенты обычно посылали этот заголовок в последнем запросе к серверу, чтобы безопасно закрыть соединение.
- Потоковая передача данных, при которой клиент может в рамках соединения посылать множественные запросы к серверу, не ожидая ответов, а сервер посылает ответы в той же последовательности, в которой получены запросы. Устанавливается заголовок Content-Length, с помощью которого клиент определяет, где заканчивается один ответ и можно ожидать следующий.
- Chunked Transfers если контент строится динамически и сервер в начале передачи не может определить Content-Length, он начинает отсылать контент частями, друг за другом, и добавлять Content-Length к каждой передаваемой части. Когда все части отправлены, посылается пустой пакет с заголовком Content-Length, установленным в 0, сигнализируя клиенту, что передача завершена. Чтобы сказать клиенту, что передача будет вестись по частям, сервер добавляет заголовок Transfer-Encoding: chunked.
- Content negotiation, including language, encoding, or type, has been introduced, and allows a client and a server to agree on the most adequate content to exchange.
- Thanks to the Host header, the ability to host different domains at the same IP address now allows server colocation.

## HTTP/2

- It is a binary protocol rather than text. It can no longer be read and created manually. Despite this hurdle, improved optimization techniques can now be implemented.

Каждый запрос и ответ HTTP/2 получает уникальный идентификатор потока и разделяется на фреймы. Фреймы представляют собой просто бинарные части данных. Коллекция фреймов называется потоком (Stream). Каждый фрейм содержит идентификатор потока, показывающий, к какому потоку он принадлежит, а также каждый фрейм содержит общий заголовок. Также, помимо того, что идентификатор потока уникален, стоит упомянуть, что каждый клиентский запрос использует нечётные id, а ответ от сервера — чётные.

- It is a multiplexed protocol. Parallel requests can be handled over the same connection, removing the order and blocking constraints of the HTTP/1.x protocol.

Поскольку HTTP/2 является бинарным протоколом, использующим для запросов и ответов фреймы и потоки, как было упомянуто выше, все потоки посылаются в едином TCP-соединении, без создания дополнительных. Сервер, в свою очередь, отвечает аналогичным асинхронным образом. Это значит, что ответ не имеет порядка, и клиент использует идентификатор потока, чтобы понять, к какому потоку принадлежит тот или иной пакет. Это решает проблему блокировки начала очереди (head-of-line blocking) — клиенту не придётся простаивать, ожидая обработки длинного запроса, ведь во время ожидания могут обрабатываться остальные запросы.

- It compresses headers. As these are often similar among a set of requests, this removes duplication and overhead of data transmitted.

Это было частью отдельного RFC, специально нацеленного на оптимизацию отправляемых заголовков. В основе лежало то, что если мы постоянного обращаемся к серверу из одного и того же клиента, то в заголовках раз за разом посылается огромное количество повторяющихся данных. А иногда к этому добавляются ещё и куки, раздувающие размер заголовков, что снижает пропускную способность сети и увеличивает время задержки. Чтобы решить эту проблему, в HTTP/2 появилось сжатие заголовков.
В отличие от запросов и ответов, заголовки не сжимаются в gzip или подобные форматы. Для сжатия используется иной механизм — литеральные значения сжимаются по алгоритму Хаффмана, а клиент и сервер поддерживают единую таблицу заголовков. Повторяющиеся заголовки (например, user agent) опускаются при повторных запросах и ссылаются на их позицию в таблице заголовков.

- It allows a server to populate data in a client cache, in advance of it being required, through a mechanism called the server push.

Server push — ещё одна потрясающая особенность HTTP/2. Сервер, зная, что клиент собирается запросить определённый ресурс, может отправить его, не дожидаясь запроса. Вот, например, когда это будет полезно: браузер загружает веб-страницу, он разбирает её и находит, какой ещё контент необходимо загрузить с сервера, а затем отправляет соответствующие запросы.

Server push позволяет серверу снизить количество дополнительных запросов. Если он знает, что клиент собирается запросить данные, он сразу их посылает. Сервер отправляет специальный фрейм PUSH_PROMISE, говоря клиенту: «Эй, дружище, сейчас я тебе отправлю вот этот ресурс. И не надо лишний раз беспокоить.» Фрейм PUSH_PROMISE связан с потоком, который вызвал отправку push-а, и содержит идентификатор потока, по которому сервер отправит нужный ресурс.

- Приоритизация запросов

Клиент может назначить приоритет потоку, добавив информацию о приоритете во фрейм HEADERS, которым открывается поток. В любое другое время клиент может отправить фрейм PRIORITY, меняющий приоритет потока.

Если никакой информации о приоритете не указанно, сервер обрабатывает запрос асинхронно, т.е. без какого-либо порядка. Если приоритет назначен, то, на основе информации о приоритетах, сервер принимает решение, сколько ресурсов выделяется для обработки того или иного потока.

## Ajax Polling:

In Ajax polling, a client makes XHR(XMLHttpRequest)/Ajax requests to server repeatedly at some regular interval to check for new data.

- A client initiates requests at a small regular intervals (e.g 0.5 Seconds)
- The server prepares the response and sends it back to to the client just like normal HTTP requests.

Making repeated requests to server wastes resources as each new incoming connection must be established, the HTTP headers must be passed, a query for new data must be performed, and a response (usually with no new data to offer) must be generated and delivered. The connection must be closed and any resources cleaned up.

## Long Polling:

As in regular polling, rather than having to repeat this process multiple times for every client until new data for a given client becomes available, Long polling is technique where the server elects to hold a client connection open for as long as possible, delivering a response only after data becomes available or timeout threshold has been reached. After receiving response client immediately sends the next request.
On the client side, only a single request to the server needs to be managed. When the response is received, the client can initiate a new request, repeating this process as many times as necessary.

- A client initiates an XHR/AJAX request, requesting some data from a server.
- The server does not immediately respond with request information but waits until there is new information available.
- When there is new information available, the server responds with new information or timeout.
- The client receives the new information and immediately sends another request to the server restarting the process.

Some challenges in long polling:

- Performance and scaling
- Device support and fallbacks
- Message ordering and delivery guarantees.
Message ordering cannot be guaranteed if the same client opens multiple connections to the server. If the client was not able to receive the message then there will be possible message loss.

## WebSockets:

WebSocket is a communication protocol which provides full-duplex communication channels over a single TCP connection.

- It is different from HTTP but compatible with HTTP.
- Works over port 80 and 443 (in case of TLS encrypted) and supports HTTP proxies and intermediaries.
- To achieve compatibility, the WebSocket handshake uses Upgrade header to update the protocol to the WebSocket protocol.

The WebSocket protocol enables interaction between a client and a web server with lesser overheads, providing real-time data transfer from and to the server. WebSockets keeps the connection open, allowing messages to be passed back and forth between the client and the server. In this way, a two-way ongoing conversation can take place between the client and the server.

A WebSocket connection flow will look something like this:

- A client initiates a WebSocket handshake process by sending a request which also contains Upgrade header to switch to WebSocket protocol along with other information.
- The server receives WebSocket handshake request and process it.
- If the server can establish the connection and agrees with client terms then sends a response to the client, acknowledging the WebSocket handshake request with other information
- If the server can not establish the connection then it sends response acknowledging it cannot establish WebSocket connection.
- Once the client receives a successful WebSocket connection handshake request, WebSocket connection will be opened. Now, client and servers can start sending data in both directions allowing real-time communication.
- The connection will be closed once the server or the client decides to close the connection.

## Server Sent Events:

Unlike WebSockets, Server-Sent Events are a one-way communication channel where events flow from server to client only. Server-Sent Events allows browser clients to receive a stream of events from a server over an HTTP connection without polling.
A client subscribes to a “stream” from a server and the server will send messages (“event-stream”) to the client until the server or the client closes the stream. It is up to the server to decide when and what to send the client, for instance as soon as data changes.

- Browser client creates a connection using an EventSource API with a server endpoint which is expected to return a stream of events over time. This essentially makes an HTTP request at given URL.
- The server receives a regular HTTP request from the client and opens the connection and keeps it open. The server can now send the event data as long as it wants or it can close the connection if there are no data.
- The client receives each event from the server and process it. If it receives a close signal from the server it can close the connection. The client can also initiate the connection close request.
