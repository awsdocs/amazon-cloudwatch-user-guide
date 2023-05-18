# SAP HANA<a name="appinsights-metrics-sap-hana"></a>

**Note**  
CloudWatch Application Insights supports only single SID HANA environments\. If multiple HANA SIDs are attached, monitoring will be set up for only the first detected SID\.

CloudWatch Application Insights supports the following metrics:

hanadb\_every\_service\_started\_status

hanadb\_daemon\_service\_started\_status

hanadb\_preprocessor\_service\_started\_status

hanadb\_webdispatcher\_service\_started\_status

hanadb\_compileserver\_service\_started\_status

hanadb\_nameserver\_service\_started\_status

hanadb\_server\_startup\_time\_variations\_seconds

hanadb\_level\_5\_alerts\_count

hanadb\_level\_4\_alerts\_count

hanadb\_out\_of\_memory\_events\_count

hanadb\_max\_trigger\_read\_ratio\_percent

hanadb\_max\_trigger\_write\_ratio\_percent

hanadb\_log\_switch\_wait\_ratio\_percent

hanadb\_log\_switch\_race\_ratio\_percent

hanadb\_time\_since\_last\_savepoint\_seconds

hanadb\_disk\_usage\_highlevel\_percent

hanadb\_max\_converter\_page\_number\_count

hanadb\_long\_running\_savepoints\_count

hanadb\_failed\_io\_reads\_count

hanadb\_failed\_io\_writes\_count

hanadb\_disk\_data\_unused\_percent

hanadb\_current\_allocation\_limit\_used\_percent

hanadb\_table\_allocation\_limit\_used\_percent

hanadb\_host\_total\_physical\_memory\_mb

hanadb\_host\_physical\_memory\_used\_mb

hanadb\_host\_physical\_memory\_free\_mb

hanadb\_swap\_memory\_free\_mb

hanadb\_swap\_memory\_used\_mb

hanadb\_host\_allocation\_limit\_mb

hanadb\_host\_total\_memory\_used\_mb

 hanadb\_host\_total\_peak\_memory\_used\_mb

hanadb\_host\_total\_allocation\_limit\_mb

hanadb\_host\_code\_size\_mb

hanadb\_host\_shared\_memory\_allocation\_mb

hanadb\_cpu\_usage\_percent

hanadb\_cpu\_user\_percent

hanadb\_cpu\_system\_percent

hanadb\_cpu\_waitio\_percent

hanadb\_cpu\_busy\_percent

hanadb\_cpu\_idle\_percent

hanadb\_long\_delta\_merge\_count

hanadb\_unsuccessful\_delta\_merge\_count

hanadb\_successful\_delta\_merge\_count

hanadb\_row\_store\_allocated\_size\_mb

hanadb\_row\_store\_free\_size\_mb

hanadb\_row\_store\_used\_size\_mb

hanadb\_temporary\_tables\_count

hanadb\_large\_non\_compressed\_tables\_count

hanadb\_total\_non\_compressed\_tables\_count

hanadb\_longest\_running\_job\_seconds

hanadb\_average\_commit\_time\_milliseconds

hanadb\_suspended\_sql\_statements\_count

hanadb\_plan\_cache\_hit\_ratio\_percent

hanadb\_plan\_cache\_lookup\_count

hanadb\_plan\_cache\_hit\_count

hanadb\_plan\_cache\_total\_execution\_microseconds

hanadb\_plan\_cache\_cursor\_duration\_microseconds

hanadb\_plan\_cache\_preparation\_microseconds

hanadb\_plan\_cache\_evicted\_count

hanadb\_plan\_cache\_evicted\_microseconds

hanadb\_plan\_cache\_evicted\_preparation\_count

hanadb\_plan\_cache\_evicted\_execution\_count

hanadb\_plan\_cache\_evicted\_preparation\_microseconds

hanadb\_plan\_cache\_evicted\_cursor\_duration\_microseconds

hanadb\_plan\_cache\_evicted\_total\_execution\_microseconds

hanadb\_plan\_cache\_evicted\_plan\_size\_mb

hanadb\_plan\_cache\_count

hanadb\_plan\_cache\_preparation\_count

hanadb\_plan\_cache\_execution\_count

hanadb\_network\_collision\_rate

hanadb\_network\_receive\_rate

hanadb\_network\_transmit\_rate

hanadb\_network\_packet\_receive\_rate

hanadb\_network\_packet\_transmit\_rate

hanadb\_network\_transmit\_error\_rate

hanadb\_network\_receive\_error\_rate

hanadb\_time\_until\_license\_expires\_days

hanadb\_is\_license\_valid\_status

hanadb\_local\_running\_connections\_count

hanadb\_local\_idle\_connections\_count

hanadb\_remote\_running\_connections\_count

hanadb\_remote\_idle\_connections\_count

hanadb\_last\_full\_data\_backup\_age\_days

hanadb\_last\_data\_backup\_age\_days

hanadb\_last\_log\_backup\_age\_hours

hanadb\_failed\_data\_backup\_past\_7\_days\_count

hanadb\_failed\_log\_backup\_past\_7\_days\_count

hanadb\_oldest\_backup\_in\_catalog\_age\_days

hanadb\_backup\_catalog\_size\_mb

hanadb\_hsr\_replication\_status

hanadb\_hsr\_log\_shipping\_delay\_seconds

hanadb\_hsr\_secondary\_failover\_count

hanadb\_hsr\_secondary\_reconnect\_count

hanadb\_hsr\_async\_buffer\_used\_mb

hanadb\_hsr\_secondary\_active\_status

hanadb\_handle\_count

hanadb\_ping\_time\_milliseconds

hanadb\_connection\_count

hanadb\_internal\_connection\_count

hanadb\_external\_connection\_count

hanadb\_idle\_connection\_count

hanadb\_transaction\_count

hanadb\_internal\_transaction\_count

hanadb\_external\_transaction\_count

hanadb\_user\_transaction\_count

hanadb\_blocked\_transaction\_count

hanadb\_statement\_count

hanadb\_active\_commit\_id\_range\_count

hanadb\_mvcc\_version\_count

hanadb\_pending\_session\_count

hanadb\_record\_lock\_count

hanadb\_read\_count

hanadb\_write\_count

hanadb\_merge\_count

hanadb\_unload\_count

hanadb\_active\_thread\_count

hanadb\_waiting\_thread\_count

hanadb\_total\_thread\_count

hanadb\_active\_sql\_executor\_count

hanadb\_waiting\_sql\_executor\_count

hanadb\_total\_sql\_executor\_count

hanadb\_data\_write\_size\_mb

hanadb\_data\_write\_time\_milliseconds

hanadb\_log\_write\_size\_mb

hanadb\_log\_write\_time\_milliseconds

hanadb\_data\_read\_size\_mb

hanadb\_data\_read\_time\_milliseconds

hanadb\_log\_read\_size\_mb

hanadb\_log\_read\_time\_milliseconds

hanadb\_data\_backup\_write\_size\_mb

hanadb\_data\_backup\_write\_time\_milliseconds

hanadb\_log\_backup\_write\_size\_mb

hanadb\_log\_backup\_write\_time\_milliseconds

hanadb\_mutex\_collision\_count

hanadb\_read\_write\_lock\_collision\_count

hanadb\_admission\_control\_admit\_count

hanadb\_admission\_control\_reject\_count

hanadb\_admission\_control\_queue\_size\_mb

hanadb\_admission\_control\_wait\_time\_milliseconds