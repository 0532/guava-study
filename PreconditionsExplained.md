# Preconditions
Guava provides a number of precondition checking utilities.  We strongly recommend importing these statically.

Each method has three variants:
  * No extra arguments.  Any exceptions are thrown without error messages.
  * An extra `Object` argument.  Any exception is thrown with the error message `object.toString()`.
  * An extra `String` argument, with an arbitrary number of additional `Object` arguments.  This behaves something like printf, but for GWT compatibility and efficiency, it only allows `%s` indicators.  Example:
```
checkArgument(i >= 0, "Argument was %s but expected nonnegative", i);
checkArgument(i < j, "Expected i < j, but %s > %s", i, j);
```

| Signature (not including extra args) | Description | Exception thrown on failure |
|:-------------------------------------|:------------|:----------------------------|
| <a href='http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/base/Preconditions.html#checkArgument(boolean)'><code>checkArgument(boolean)</code></a> | Checks that the `boolean` is `true`.  Use for validating arguments to methods.  | `IllegalArgumentException`  |
| <a href='http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/base/Preconditions.html#checkNotNull(T)'><code>checkNotNull(T)</code></a> | Checks that the value is not null.  Returns the value directly, so you can use `checkNotNull(value)` inline. | `NullPointerException`      |
| <a href='http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/base/Preconditions.html#checkState(boolean)'><code>checkState(boolean)</code></a> | Checks some state of the object, not dependent on the method arguments.  For example, an `Iterator` might use this to check that `next` has been called before any call to `remove`. | `IllegalStateException`     |
| <a href='http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/base/Preconditions.html#checkElementIndex(int, int)'><code>checkElementIndex(int index, int size)</code></a> | Checks that `index` is a valid _element_ index into a list, string, or array with the specified size.  An element index may range from 0 inclusive to `size` **exclusive**.  You don't pass the list, string, or array directly; you just pass its size.<br>Returns <code>index</code>. |`IndexOutOfBoundsException ` |
| <a href='http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/base/Preconditions.html#checkPositionIndex(int, int)'><code>checkPositionIndex(int index, int size)</code></a> | Checks that <code>index</code> is a valid <i>position</i> index into a list, string, or array with the specified size.  A position index may range from 0 inclusive to <code>size</code> <b>inclusive</b>.  You don't pass the list, string, or array directly; you just pass its size.<br>Returns <code>index</code>. |<code>IndexOutOfBoundsException</code> |
| <a href='http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/base/Preconditions.html#checkPositionIndexes(int, int, int)'><code>checkPositionIndexes(int start, int end, int size)</code></a> | Checks that <code>[start, end)</code> is a valid sub range of a list, string, or array with the specified size.  Comes with its own error message. | <code>IndexOutOfBoundsException</code> |</tbody></table>

We preferred rolling our own preconditions checks over e.g. the comparable utilities from Apache Commons for a few reasons. Briefly:

<ul><li>After static imports, the Guava methods are clear and unambiguous.  <code>checkNotNull</code> makes it clear what is being done, and what exception will be thrown.<br>
</li><li><code>checkNotNull</code> returns its argument after validation, allowing simple one-liners in constructors: <code>this.field = checkNotNull(field)</code>.<br>
</li><li>Simple, varargs "printf-style" exception messages.  (This advantage is also why we recommend continuing to use <code>checkNotNull</code> over <a href='http://docs.oracle.com/javase/7/docs/api/java/util/Objects.html#requireNonNull(java.lang.Object,java.lang.String)'>Objects.requireNonNull</a> introduced in JDK 7.)</li></ul>

We recommend that you split up preconditions into distinct lines, which can help you figure out which precondition failed while debugging.  Additionally, you should provide helpful error messages, which is easier when each check is on its own line.
