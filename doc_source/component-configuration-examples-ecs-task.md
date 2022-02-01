# Amazon ECS task<a name="component-configuration-examples-ecs-task"></a>

The following example shows a component configuration in JSON format for Amazon ECS task\.

```
{
   "logs":[
      {
         "logGroupName":"/ecs/my-task-definition",
         "logType":"APPLICATION",
         "monitor":true
      }
   ]
}
```