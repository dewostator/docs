# Мониторинг {{ sk-hybrid-name }} 

{{ sk-hybrid-name }} собирает и хранит метрики в формате [Prometheus](https://prometheus.io). Метрики {{ sk-hybrid-name }} доступны по URL-адресу: `<IP-адрес>:17002/metrics/prometheus`, где `<IP-адрес>` — IP-адрес сервиса {{ sk-hybrid-name }} в вашей сети.

## Общие метрики {{ sk-hybrid-name }} {#metrics}

Собираемые метрики различаются для синтеза и распознавания речи. Общей для сервисов является метрика `TYPE grpc_code_*` — коды ответов протокола gRPC. См. [документацию gRPC](https://grpc.github.io/grpc/core/md_doc_statuscodes.html).

## Метрики синтеза речи {#tts-metrics}

* `synthesis_sps` — количество секунд синтезированного текста, которое генерируется за секунду работы.
* `synthesis_latency` — [гистограмма](https://prometheus.io/docs/practices/histograms/) распределения времени ответа в миллисекундах.

## Метрики распознавания речи {#stt-metrics}

* `in_flight_requests` — количество активных сессий.
* `recognize_sps` — количество секунд распознанной речи, которое было обработано за секунду работы.
* `recognize_rtf` — гистограмма распределения [Real Time Factor](https://devopedia.org/speech-recognition). Описывает отношение времени распознавания к длительности распознаваемого текста, выражена в процентах. Значение больше 100 означает, что сервер не справляется с потоком запросов.