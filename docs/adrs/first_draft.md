## Основные компонентны системы:

Внутренние решения:

1. Client UI - фронтенд, через который клиент сможет:
    * Войти в систему;
    * Создать заказ, используя форму [US-020];
    * Просмотреть созданные заказы [US-010];
    * Изменить статус заказа [US-030];
    * Отменить/перенести заказ [US-112];
    * Прикрепить способ оплаты [US-190];
    * Посмотреть историю списаний [US-220];

2. Manager UI - фронтенд через который менеджер может:
    * Создавать/редактировать доступные клиентам типы услуг [US-021];
    * Получать информацию (включая номера телефонов) о выполненных/отмененных/провальных заказах клиента [US-160/US-170];
    * Начислять любое количество денег для выбранного воркера [US-240];
    * Добавлять воркера в пул воркеров [US-100];

3. Worker UI - фронтенд через который исполнитель задания может:
    * Отмечаться на заказе, слать отчет с подписью клиента [US-070];
    * Просматривать список активных/завершенных/отмененных заказов [US-120];
    * Посмотреть историю списаний [US-220];

4. Warehouse Employee UI - фронтенд через который сотрудник сборки расходников:
    * Получит информацию о необходимых для заказа расходниках [US-130/US-140];
    * Пометит заказ как готовый к тому, чтобы расходники были забраны воркером [US-130/US-140];
    * Внесет информацию в систему инвентаризации (???);

5. B2B Applications UI - фронтенд через который воркеры:
    * Заполняют анкету;
    * Проходят OTP аутентификацию [US-081];
    * Отправляют заявку чтобы стать воркером [US-080];

6. Order Service - сервис отвечающий за информацию о заказах:
    * CRUD над сущностью заказа;
    * Инициация/сопровождение бизнес-флоу заказа в Camunda/Temporal/Etc;
    * Обновление статуса заказа;
    * Отправка события с нотификациями по заказу в Notification Center e.g воркер подобран, флоу изменение статуса, подтверждение от клиента и воркера, необходиомсть забрать воркеру пакет с расходниками.

7. Matching Service - система матчинга заказов и воркеров:
    * Получение задачи о необходимости подбора воркера для заказа из очереди сообщений (DataBus);
    * Получение информации о воркерах из сервиса Worker;
    * Рандомно связывает воркера и клиента, без специфики [US-060];
    * Отправка события в очередь сообщений о подобранном к заказу воркере;
    * Отправка события с нотификациями по заказу в Notification Center e.g воркер был подобран [US-276];

8. Price Service - логика расчета цены за услуги на основе фичей заказа.
    * Получает сообщение из очереди сообщений с задачей расчитать стоимость выполнения заказа;
    * Получает информацию о суммарных тратах клиента в сервисе заказов;
    * Если клиент заплатил больше 10к условных кото-единиц в сумме, ему даётся скидка в 5%, если больше 20к — скидка 6%. За каждые 10к скидка увеличивается на 1%, предел скидки — 12% [US-040];
    * Задает стоимость в 100 единиц [US-050];

9. Billing Service - система выставления инвойсов и списаний:
    * Интегрирована с системой платежей Gold Hat;
    * В расчетный период осуществляет выставление инвойсов и списания для клиента и воркера [US-190/US-200/US-210];
    * Отправка события с нотификациями по заказу в Notification Center e.g инвойс для клиента был сгенерирован, статус оплаты для клиентов, оплаченные инвойсы для воркеров [US-274/US-275]
    * Отправка события в очередь сообщений о выплаченном инвойсе;

10. Worker Service - сервис для работы с сущностью воркера:
    * CRUD над пулом воркеров;

11. Employee Service - сервис для сотрудников сборки:
    * Получение списка заказов и информации о необходимых для них расходников;
    * Отправка события о готовности расходников для заказа;
    * Отправка события с нотификациями по заказу в Notification Center e.g расходники были подобраны [US-273];

12. OrderBet Servie - сервис мотивации менеджеров с возможностью делать ставки.

13. Manager QA Service - сервис контроля качества за заказом:
    * Хранит репорты менеджеров;
    * Отправка события с нотификациями по заказу в Notification Center e.g контроль качества был проведен;


?? Не забыть про аудит лог начислений руками

Внешние решения:

1. IAM - система для авторизациии/аутентификации в приложении. На основе JWT токена авторизует или нет запрос.
2. Process Orchestration Platform System e.g Temporal, Camunda - оркестрация сложных бизнес процессов.
3. Notification Center - система для рассылок по email.
4. Gold Hat - система денежных расчетов.
5. For-tune system - изготовители печене.