# Overview #

`#include <time.h>`

## Time in seconds ##

unix `time()` - just the same old true unix time(). not recommended, of course

## Time in microseconds ##

`bigtime_t hal_system_time(void);` - useconds from kernel boot

`bigtime_t hal_local_time(void);` - absolute time in useconds - **NB! Base?**


## Sleep ##

`void hal_sleep_msec( int miliseconds );` - sleep (by switching to another thread) for given time. (In **hal.h**)

`void phantom_spinwait(int millis);` - wait some mseconds by spinning (not giving CPU away) - note that you have to disable interrupts to wait EXACTLY for given time.
 
`void tenmicrosec(void);` - wait by spinning, ~10 microsec


## Polled timeouts ##

```
typedef bigtime_t polled_timeout_t;

void set_polled_timeout( polled_timeout_t *timer, bigtime_t timeout_uSec );
bool check_polled_timeout( polled_timeout_t *timer );
```

Use set_polled_timeout() to setup timeout (in microseconds). Use check_polled_timeout() to find out if time is passed. Note that interrupts must be enabled. This call depends on system timer interrupt granularity. (10 msec?)

## Timed calls (callouts) ##

Call function after timeout from timer interrupt.

```
void phantom_request_timed_call( timedcall_t *entry, u_int32_t flags );
void phantom_request_timed_func( timedcall_func_t f, void *arg, int msecLater, u_int32_t flags );
```

timedcall\_t is:

`timedcall_func_t 	f;       ` - func to call
`void                *arg;     ` - arg to pass to f
`long                msecLater;` - milliseconds to pass before call

flags are:

  * TIMEDCALL\_FLAG\_PERIODIC   - func will be called periodically
  * TIMEDCALL\_FLAG\_AUTOFREE   - entry will be freed automatically

**NB!** Call is done in timer interrupt! Be quick! Don't attempt to sleep (mutex/cond) or call functions that sleep.

**NB!** It is possible for callout to happen BEFORE the return from this function.
```
void phantom_request_cond_signal( int msec, hal_cond_t *cond);
void phantom_request_cond_broadcast( int msec, hal_cond_t *cond);
```

**TODO**

`void phantom_request_timed_thread_wake( int tid, long timeMsec );`

Implement copy of these, but working on separate thread.

## Net Timer ##

Name is historical. Call function agfter timeout. Function is called in thread context, so
it can call functions that block briefly (such as malloc/free). Still you can't sleep for long.

```
int set_net_timer(net_timer_event *e, unsigned int delay_ms, net_timer_callback callback, void *args, int flags);
int cancel_net_timer(net_timer_event *e);
```

