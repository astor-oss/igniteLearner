[https://mail.google.com/mail/u/0/\#inbox/15fbfd6e32a4321e](https://mail.google.com/mail/u/0/#inbox/15fbfd6e32a4321e)



![](/assets/Event.png)

```
import org.apache.ignite.Ignite;
```

```
import org.apache.ignite.IgniteCache;
import org.apache.ignite.Ignition;
import org.apache.ignite.cache.query.SqlQuery;
import org.apache.ignite.cache.query.annotations.QuerySqlField;
import org.apache.ignite.configuration.CacheConfiguration;
import org.apache.ignite.events.CacheQueryExecutedEvent;
import org.apache.ignite.events.EventAdapter;
import org.apache.ignite.events.EventType;
import org.apache.ignite.lang.IgnitePredicate;

public class QueryExecutedEvent {

    static class MyValue {
        @QuerySqlField(index = true)
        private int search;
    }

    public static void main(String[] args) {
        try (Ignite ignite = Ignition.start("example-ignite.xml")) {
            IgnitePredicate
<
EventAdapter
>
 locLsnr = new IgnitePredicate
<
EventAdapter
>
()
            {
                @Override
                public boolean apply(EventAdapter evt) {
                    switch (evt.type()) {

                        case EventType.EVT_CACHE_QUERY_EXECUTED:
                            CacheQueryExecutedEvent cacheExecEvent = (CacheQueryExecutedEvent)evt;
                            String clause = cacheExecEvent.clause();
                            System.out.println("query "+clause);
                            break;
                        case EventType.EVT_CACHE_QUERY_OBJECT_READ:
                            CacheQueryExecutedEvent cacheObjReadEvent =
                                (CacheQueryExecutedEvent)evt;
                            String clause2 = cacheObjReadEvent.clause();
                            System.out.println("query "+clause2);
                            break;
                        default:
                            System.out.println(evt.getClass());
                            break;
                    }

                    return true;
                }
            };

            ignite.events().localListen(locLsnr,
                EventType.EVT_CACHE_QUERY_EXECUTED, EventType.EVT_CACHE_QUERY_OBJECT_READ);

            CacheConfiguration
<
Object, Object
>
 cfg = new CacheConfiguration
<
>
("DFLT");

            cfg.setIndexedTypes(Integer.class, MyValue.class);

            IgniteCache
<
Object, Object
>
 cache = ignite.createCache(cfg);


            SqlQuery query = new SqlQuery(MyValue.class, "search = ?");
            query.setArgs(5);
            query.setLocal(true);

            cache.query(query).getAll();
        }
    }
}
```



