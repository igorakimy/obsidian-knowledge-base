Карта основана на структуре данных *хеш-таблица*. 

**Хеш-таблица** представляет собой массив сегментов, каждый из которых является указателем на массив пар "ключ - значение".

Если заранее известно количество элементов в карте, то нужно создавать её с указанием начального размера. Это потенциально может увеличить производительность до 60%, т.к. не будут задействованы дорогостоящие операции создания новых сегментов(buckets) карты.

Карта в Go - это указатель на структуру runtime.hmap: 
```go
type hmap struct {  
    // Note: the format of the hmap is also encoded in cmd/compile/internal/reflectdata/reflect.go.    // Make sure this stays in sync with the compiler's definition.    count     int // # live cells == size of map.  Must be first (used by len() builtin)  
    flags     uint8  
    B         uint8  // log_2 of # of buckets (can hold up to loadFactor * 2^B items)  
    noverflow uint16 // approximate number of overflow buckets; see incrnoverflow for details    hash0     uint32 // hash seed  
  
    buckets    unsafe.Pointer // array of 2^B Buckets. may be nil if count==0.  
    oldbuckets unsafe.Pointer // previous bucket array of half the size, non-nil only when growing  
    nevacuate  uintptr        // progress counter for evacuation (buckets less than this have been evacuated)  
  
    extra *mapextra // optional fields  
}
```
#### Особенности карты
- является указателем на структуру *runtime.hmap*;
- состоит из восьмиэлементных сегментов(buckets);
- когда увеличивается в размере, удваивает количество своих сегментов, происходит т.н. *эвакуация данных*;
- количество сегментов(buckets) в памяти остается прежним при удалении всех элементов из карты, потому как сегменты просто очищаются, но не удаляются;
- карта может только расти и никогда не уменьшается, благодаря предыдущему пункту.
- порядок обхода при итерации является случайным;
#### Решение некоторых проблем при работе с картой
- если заранее известно кол-во элементов, то необходимо указывать его при инициализации карты;
- регулярно повторно создавать копии текущей карты и освобождать предыдущую, чтобы обнулять таким образом кол-во созданных сегментов;
- изменить тип карты для хранения указателя, таким образом каждый сегмент будет резервировать память в соответствии с размером указателя (то есть 8 байтов в 64-разрядных и 4 байта в 32-разрядных системах), что существенно сэкономит память.