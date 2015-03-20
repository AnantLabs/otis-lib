# Introduction #

Very often, there is a need to map a property to an expression which is a chain of property calls (forget Law of Demeter for the moment) or to a method call with the unknown implementation.

The problem is that in expression like `$Context.CurrentUser.FirstName`, `CurrentUser` can be null, so that call to `FirstName` property will throw an `NullReferenceException`.

The same thing can happen when you map to a method: `$Context.SomeComplexMethodWhichMightThrow()`.


# Proposal #

A mapping expression could be decorated with attribute specifying what should be done in case if `NullReferenceException` is thrown:

`<member name="TargetProp" expression="$Prop1.Prop2" onNullReferenceException="[null]" />`

Then the mapping code generated by Otis would be enclosed in `try-catch` block which would catch `NullReferenceException` exception and set appropriate value to target property.


# Extensions #

This approach might be extended to other exceptions as well, if necessary.