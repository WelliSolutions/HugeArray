# HugeArray
Arrays for .NET that can exceed the size limitations of the .NET built-in arrays.

## Motivation

There are some limitation on usual .NET arrays.

* One limitation is the indexer of `System.Array`, which is defined as `int` and thus is a 32 bit value. Obviously it should not be negative either, so we have a maximum of 2^31 possible values to put into the indexer. 

* Another limitation is that the maximum size of a single object in .NET is also 2 GB. 

## Research

* Interestingly, there is a [`SetValue()` overload that takes an Int64](https://docs.microsoft.com/en-us/dotnet/api/system.array.setvalue?view=netframework-4.8#System_Array_SetValue_System_Object_System_Int64_), which tells us that on the level of IL code, the 32 bit limits might not exist.
* The idea of having a huge array is not new. In 2005 already, *joshwil* published an article called [BigArray, getting around the 2 GB array size limit (Microsoft Blog archive)](https://docs.microsoft.com/en-us/archive/blogs/joshwil/bigarrayt-getting-around-the-2gb-array-size-limit). Unfortunately, I could not find out under which license that code was published, so I won't reuse it.
  * Also, the given code does not implement all methods that normal arrays have, like [`CopyTo()`](https://docs.microsoft.com/en-us/dotnet/api/system.array.copyto?view=netframework-4.8) which I'd like to have for my intended use
* There seems to be a need for such huge arrays, as we can see in some StackOverflow posts:
  * [What is the maximum size that an array can hold?]([What is the Maximum Size that an Array can hold?](https://stackoverflow.com/questions/1391672/what-is-the-maximum-size-that-an-array-can-hold)) from 2009
  * [Single objects still limited to 2 GB in CLR 4.0?]([Single objects still limited to 2 GB in size in CLR 4.0?](https://stackoverflow.com/questions/1087982/single-objects-still-limited-to-2-gb-in-size-in-clr-4-0)) from 2009
  * [What is the maximum length of an array in .NET on 64-bit Windows](https://stackoverflow.com/questions/2338778/what-is-the-maximum-length-of-an-array-in-net-on-64-bit-windows) from 2010
  * [My own request on Software Recommendations](https://softwarerecs.stackexchange.com/questions/36175/net-library-that-simulates-very-large-byte-array) from 2016 was not answered yet
* With .NET 4.5, there is a setting [`gcAllowVeryLargeObjects` ](https://docs.microsoft.com/en-us/dotnet/framework/configure-apps/file-schema/runtime/gcallowverylargeobjects-element) which you put into your `exe.config` file. The maximum number of elements is then 2^32 and with multi-dimensional arrays, you can exceed the  2 GB limit. This still requires you to adapt to these limitations and access a 2D array where you just wanted to have a one-dimensional array.

## Goals

* The use of HugeArray should be straight forward. Anyone who knows how to use an array should be able to use HugeArray as well.
* HugeArray shall be a managed implementation only. I don't want to rely on native code, C++ CLI, P/Invoke or unsafe code.
* I am still coding for .NET Framework primarily, so this library will target .NET framework as well, not .NET Core.
* I won't focus on performance. I'll see whether it's fast enough or not.
* HugeArray is an intermediate step for me. What I really need is a huge circular buffer, potentially containing a sort of file system. This will be one of the use cases to see whether HugeArray can be used as expected.
