# RxJavaStackTracer
RxJava is not good when it comes to stack-traces. When we use `observeOn` or `subscribeOn` we get our stack-trace from the Rx process root, erasing all the call history.

`RxJavaStackTracer` seems to help. Especially for the existing projects. Because it provides no code invasion.

## Usage
Register [RxJavaStackTracer](RxJavaStackTracer/app/src/main/java/stacktracer/rxjava/cookizz/com/rxjavastacktracer/RxJavaStackTracer.java) to RxJavaPlugins during creation of you app **only once**.

	public class MyApp extends Application {
		@Override
		public void onCreate() {
			RxJavaPlugins.getInstance()
					.registerObservableExecutionHook(new RxJavaStackTracer());
		}
	}

After that, stack-trace will involve all the call history everywhere you subscribe an Observable even if you change thread using observeOn(), subscribeOn().	

## Thanks
The inner static class `OperatorTraceOnError` comes from [konmik](https://github.com/konmik)'s solution in [RxJava issue #3521](https://github.com/ReactiveX/RxJava/issues/3521).