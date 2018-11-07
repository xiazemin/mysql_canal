```
$vi conf/logback.xml

      &lt;root level="WARN"&gt;

            &lt;appender-ref ref="STDOUT"/&gt;

            &lt;appender-ref ref="CANAL-ROOT" /&gt;

    &lt;/root&gt;
```

改成

```
   &lt;root level="INFO"&gt;

            &lt;appender-ref ref="STDOUT"/&gt;

            &lt;appender-ref ref="CANAL-ROOT" /&gt;

    &lt;/root&gt;
```



