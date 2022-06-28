# Домашнее задание к занятию "10.01. Зачем и что нужно мониторить"

***
### 1. Вас пригласили настроить мониторинг на проект. На онбординге вам рассказали, что проект представляет из себя платформу для вычислений с выдачей текстовых отчетов, которые сохраняются на диск. Взаимодействие с платформой осуществляется по протоколу http. Также вам отметили, что вычисления загружают ЦПУ. Какой минимальный набор метрик вы выведите в мониторинг и почему?
***
### Ответ:

Выводить для анализа неолбходимо те метрики, которые в дальнейшем помогут для принятия решения. Надо понимать, что можно выводить метрики, которые нужны руководству, а есть метрики, которые нужны тех. специлисту. В первом случае для руководства важно видеть результат работы орагнизации, т.е. для руководства можно вывести 
 - I. количетсво отчетов за настраиваемый промежуток времени (объем работы так сказать), 
 - II. количество запросов к серверу для общего понимания - используется ли платформа пользователями или нет 3. возможно, какие то данные из представленных отчетов (количетсво продаж, товаров и пр.) - то, что потребуется руководству (это необходимо дополнительно уточнить).

Второй случай, когда мониторинг настраивается для себя, как для тех. специалиста, который должен (по условия задачи) обязательно анализировать: 
 - I. Количество поступающих запросов с разбивкой на коды ответа (404, 503, 500, 200 и пр. при необходимости) - это нужно для понимания, справляется наш web-сервер с нагрузкой или нет.
 - II. Загруженность ширины канала - опять же понимать - справляемся мы с потоком обращающихся пользователей или нет. 
 - III. Метрики по диску - среднее время ответа диска, время простоя, метрики записи и чтения на диск - это необходимо для понимания справляется ли наша дисковая подсистема с нагрузкой, которая есть в текущий момент. Так же можно построить аналогичные графики по диску за некий промежуток времени, например за неделю-две, чтобы показать руководству, например, необходимость в увеличении производительности дисковой подсистемы (наприемр, вместо HDD поставить SSD). 
 - IV. Количество свободных inode - это требуется для понимания фактической нагрузки на файловую подсистему. В случае наблюдения низкого процента свободных inode их можно будет увеличить, избежав замедления работы с файловой подсистемой.
 - V. Метрики по процессору - max и min значение за период, средняя нагрузка на процессор за определенное время (CPU load average), время простоя (работа без нагрузки) - для понимания необходимости увеличения мощности (добавления ядер) при сверх высоких нагрузках при обработке текстовых ответов (из задания) или понимания в какой момент времени чаще у нас идет работа пользователей (утро, обед, вечер).
 - VI. Метрики по оперативной памяти, чтобы было понимание, что мы "не упираемся в потолок" и нам нет необходимости добавлять еще RAM для увеличения производительности сервера.

Это минимальный набор метрик, по моему. Ими можно оперировать как в момент времени, так и в какой либо промежуток и уже делать какие то выводы и умозаключения. В дальнейшем можно добавить что-то по безопасности (например, мониторинг сертификата, который будем использовать для доступа к сайту, если у нас будет в дальнейшем httpS).
***
### 2. Менеджер продукта посмотрев на ваши метрики сказал, что ему непонятно что такое RAM/inodes/CPUla. Также он сказал, что хочет понимать, насколько мы выполняем свои обязанности перед клиентами и какое качество обслуживания. Что вы можете ему предложить?
***
### Ответ:

Т.к. менеджеру продукта нет необходимости вникать в технические детали, ему надо понимать с менеджерской стороны как у него идёт бизнес. Я могу предложить для решения задачи по определию исполнения обязательств со стороны компании выводить статистику, которая будет показывать исполнение тех показателей, который прописаны в соглашении между сторонами (SLO, SLI, SLA), например, ```показатель доступности сайта``` (SLO). Для решения это задачи можно взять все ответы web-сервера и разделить их на успешные и не успешные. Если, наприемр, показатель доступности нашего ресурса по соглашению будет не менее 90% - то выставить alerting на достижение, допустим 80% и 95% от данного показателя. Тем самым менеджер будет видеть, что сайт у нас доступен и показатели по соглашению выполняются. 

Можно для менеджера показать ```качество обслуживания``` (SLI). Можно вывести так же процент корректных ответов пользователю (ответ 200 ОК). Просто некая положительная статистика, которая покажет насолько хорошо идут дела у компании.

### 3. Вашей DevOps команде в этом году не выделили финансирование на построение системы сбора логов. Разработчики в свою очередь хотят видеть все ошибки, которые выдают их приложения. Какое решение вы можете предпринять в этой ситуации, чтобы разработчики получали ошибки приложения?

### Ответ:

Ну вариантов можно предложить всё же несколько. Например всё же пойти к руководству и доказать, что мониторинг - это одна из важнейших компонент для функционирования его же бизнеса, и что если не будет мониторинга, то проблемы могут быть намного серьезнее в плоть до полной остановки бизнес процессов в организации, по этому лучше не экономить на данном направлении.

Вариант похуже - проинформировать руководство (в принципе и так проинформировали, но руководство не приняло во внимание наши доводы), далее дожадться всё же критической ситуации (если она будет) и показать, что всё же надо строить систему мониторинга, чтобы такие ситуации не случались (это ужасный пример, это больше минус не руководству, а мне будет, как специалисту, и всякие отговорки, что я не смог доказать - что это надо - не пройдут. Скажут, надо было лучше доказывать...). Следовательно разработчики наших приложений тоже будут не в восторге.

Так же есть вариант, чтобы разработчики самостоятельно собирали метрики и мониторили всё на своей стороне (не сказано по условию, что у нас изолированная система и мы не можем куда то что-то отправлять). 

Можно развернуть бесплатные системы, в связке, например, grafana+prometeus которые будут в большей мере удовлетворять потребности в сборе и отображению статистики. Так же в grafan`е есть уведомления по событиям, которые можно настроить для уведомления разработчиков. 

В общем всё бы свелось к тому, что можно при большом желании развернуть бесплатную систему сбора логов и мониторинга, которая бы могла удовлетворять первоначальные потребности в мониторинге и какое то время хранить логи для дальнейшего анализа. Ну а в дальнейшем, всё же идти к руководству и доказывать, что для системы мониторинга нужны мощности и что надо покупать для этого новое железо.
***
### 4. Вы, как опытный SRE, сделали мониторинг, куда вывели отображения выполнения SLA=99% по http кодам ответов. Вычисляете этот параметр по следующей формуле: summ_2xx_requests/summ_all_requests. Данный параметр не поднимается выше 70%, но при этом в вашей системе нет кодов ответа 5xx и 4xx. Где у вас ошибка?
***
### Ответ:

Мы допустили ошибку в вычислении качества обслуживания (SLI). Корректная формула в числителе содержит сумму 200-х ```и 300-х ответов```, разделенную на общее количество всех отданных ответов. Следовательно в нашей формуле по заданию мы не учли 300-ые ответы, в связи с чем и получили заниженный показатель.

Корректая формула:

#### (summ_2xx_requests + summ_3xx_requests)/(summ_all_requests)
