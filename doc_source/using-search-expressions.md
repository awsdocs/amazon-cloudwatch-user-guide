# Using search expressions in graphs<a name="using-search-expressions"></a>

Search expressions are a type of math expression that you can add to CloudWatch graphs\. Search expressions enable you to quickly add multiple related metrics to a graph\. They also enable you to create dynamic graphs that automatically add appropriate metrics to their display, even if those metrics don't exist when you first create the graph\.

For example, you can create a search expression that displays the `AWS/EC2 CPUUtilization` metric for all instances in the Region\. If you later launch a new instance, the `CPUUtilization` of the new instance is automatically added to the graph\.

When you use a search expression in a graph, the search finds the search expression in metric names, namespaces, dimension names, and dimension values\. You can use Boolean operators for more complex and powerful searches\.

You can't create an alarm based on the **SEARCH** expression\. This is because search expressions return multiple time series, and an alarm based on a math expression can watch only one time series\.

**Topics**
+ [CloudWatch search expression syntax](search-expression-syntax.md)
+ [CloudWatch search expression examples](search-expression-examples.md)
+ [Creating a CloudWatch graph with a search expression](create-search-expression.md)