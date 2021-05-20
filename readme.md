## Observable
- Observable
Observables are lazy Push collections of multiple values. They fill the missing spot in the following table:

        SINGLE	    MULTIPLE
Pull	Function	Iterator
Push	Promise	    Observable

## main differences between the Promise and the Observable are as follows:
- the Promise is eager, whereas the Observable is lazy,
- the Promise is always asynchronous, while the Observable can be either asynchronous or synchronous,
- the Promise can provide a single value, whereas the Observable is a stream of values (from 0 to multiple values),
- you can apply RxJS operators to the Observable to get a new tailored stream.