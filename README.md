# DuckSharp

DuckSharp is a duck-typing library for C#. This allows you to specify
that classes implement interfaces at runtime, allowing you to use them
in way previously unexpected.

Let's say that you have an interface of String-like things with
ToUpper() and ToLower()

    public interface IStringLike {
      String ToUpper();
      String ToLower();
    }

If I wanted to pass a String in, I'd be out of luck. DuckSharp changes
that. At runtime I can build a wrapper around String that appears to
implement IStringLike, allowing me to use a String in places I might
not be able to otherwise.

    String HelloWorld = "Hello World";
    IStringLike LikeHelloWorld = DuckProxy.Create(HelloWorld, typeof(IStringLike));

The library is even smart enough to recognize when DuckProxy wrappers
need to be added or removed.

    public interface ISpeech {}

    public interface IPirate {
      ISpeech Talk();
    }

    public class Ninja {
      String Talk() {
        // ...
      }
    }

    ISpeech Speech = DuckProxy.Create(new Ninja(), typeof(IPirate)).Talk();


