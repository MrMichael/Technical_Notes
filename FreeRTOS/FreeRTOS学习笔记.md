
# FreeRTOS的5个主要部分
1. 任务管理
1. 队列管理
1. 中断管理
1. 资源管理
1. 内存管理

FreeRTOS API官方说明
https://www.freertos.org/a00106.html
---
# 目录
[toc]
# 第一章 任务管理

在 FreeRTOS 中，每个执行线程都被称为”任务”。

## 本章的目的
- FreeRTOS 如何为各任务分配处理时间。
- FreeRTOS 如何选择任务投入运行。
- 任务优先级如何影响系统行为。
- 任务存在哪些状态。

#### 以及
-  如何实现一个任务。
-  如何创建一个或多个任务的实例。
-  如何使用任务参数。
-  如何改变一个已创建任务的优先级。
-  如何删除任务。
-  如何实现周期性处理。
-  空闲任务何时运行，可以用来干什么。

## 任务函数
任务是由 C 语言函数实现的。唯一特别的只是任务的函数原型，其必须返回 void，
而且带有一个 void 指针参数。
> void ATaskFunction( void *pvParameters );

FreeRTOS 任务不允许以任何方式从实现函数中返回——它们绝不能有一条”return”语句，也==不能执行到函数末尾==。如果一个任务不再需要，可以显式地将其删除。

一个任务函数可以用来创建若干个任务——创建出的任务均是独立的执行实例，拥有属于自己的栈空间，以及属于自己的自动变量(栈变量)，即任务函数本身定义的变量。

```
void ATaskFunction( void *pvParameters )
{
    /* 可以像普通函数一样定义变量。用这个函数创建的每个任务实例都有一个属于自己的iVarialbleExample变量。但如果iVariableExample被定义为static，这一点则不成立 – 这种情况下只存在一个变量，所有的任务实例将会共享这个变量。 */
    int iVariableExample = 0;
    /* 任务通常实现在一个死循环中。 */
    for( ;; )
    {
        /* 完成任务功能的代码将放在这里。 */
    }
    /* 如果任务的具体实现会跳出上面的死循环，则此任务必须在函数运行完之前删除。传入NULL参数表示删除的是当前任务 */
    vTaskDelete( NULL );
}
```

## 任务状态

1. 运行状态
2. 非运行状态

当某个任务处于运行态时，处理器就正在执行它的代码。当一个任务处于非运行态
时，该任务进行休眠，它的所有状态都被妥善保存，以便在下一次调试器决定让它进入
运行态时可以恢复执行。


## 创建任务

创建任务使用 FreeRTOS 的 API 函数 xTaskCreate()。


```
portBASE_TYPE xTaskCreate(  pdTASK_CODE pvTaskCode,     //指向任务的实现函数的指针
                            const signed portCHAR * const pcName,   //具有描述性的任务名
                            unsigned portSHORT usStackDepth,   //usStackDepth 值用于告诉内核为它分配多大的栈空间,栈空间可以保存多少个字(word)
                            void *pvParameters,         //pvParameters 的值即是传递到任务中的值。
                            unsigned portBASE_TYPE uxPriority,  //任务执行的优先级。优先级的取值范围可以从最低优先级 0 到最高优先级(configMAX_PRIORITIES – 1)。
                            xTaskHandle *pxCreatedTask );   //pxCreatedTask 用于传出任务的句柄。比如改变任务优先级，或者删除任务。
                                                            //1. pdTRUE:表明任务创建成功
                                                            //2. errCOULD_NOT_ALLOCATE_REQUIRED_MEMORY由于内存堆空间不足， FreeRTOS 无法分配足够的空间来保存任务结构数据和任务栈，因此无法创建任务。
                        
```

#### 创建任务示例

本例演示了创建并启动两个任务的必要步骤。这两个任务只是周期性地打印输出字
符串，采用原始的空循环方式来产生周期延迟。两者在创建时指定了相同的优先级，并
且在实现上除输出的字符串外完全一样。


```
void vTask1( void *pvParameters )
{
    const char *pcTaskName = "Task 1 is running\r\n";
    volatile unsigned long ul;
    /* 和大多数任务一样，该任务处于一个死循环中。 */
    for( ;; )
    {
        /* Print out the name of this task. */
        vPrintString( pcTaskName );
        /* 延迟，以产生一个周期 */
        for( ul = 0; ul < mainDELAY_LOOP_COUNT; ul++ )
        {
            /* 这个空循环是最原始的延迟实现方式。在循环中不做任何事情。后面的示例程序将采用
            delay/sleep函数代替这个原始空循环。 */
        }
    }
}

void vTask2( void *pvParameters )
{
    const char *pcTaskName = "Task 2 is running\r\n";
    volatile unsigned long ul;
    /* 和大多数任务一样，该任务处于一个死循环中。 */
    for( ;; )
    {
        /* Print out the name of this task. */
        vPrintString( pcTaskName );
        /* 延迟，以产生一个周期 */
        for( ul = 0; ul < mainDELAY_LOOP_COUNT; ul++ )
        {
            /* 这个空循环是最原始的延迟实现方式。在循环中不做任何事情。后面的示例程序将采用
            delay/sleep函数代替这个原始空循环。 */
        }
    }
}

int main( void )
{
    /* 创建第一个任务。 需要说明的是一个实用的应用程序中应当检测函数xTaskCreate()的返回值，以确保任务创建成功。 */
    xTaskCreate( vTask1, /* 指向任务函数的指针 */
                "Task 1", /* 任务的文本名字，只会在调试中用到 */
                1000, /* 栈深度 – 大多数小型微控制器会使用的值会比此值小得多 */
                NULL, /* 没有任务参数 */
                1, /* 此任务运行在优先级1上. */
                NULL ); /* 不会用到任务句柄 */
    /* Create the other task in exactly the same way and at the same priority. */
    xTaskCreate( vTask2, "Task 2", 1000, NULL, 1, NULL );
    /* 启动调度器，任务开始执行 */
    vTaskStartScheduler();
    /* 如果一切正常， main()函数不应该会执行到这里。但如果执行到这里，很可能是内存堆空间不足导致空闲任务无法创建。第五章有讲述更多关于内存管理方面的信息 */
    for( ;; );
}

```

#### 使用任务参数示例

与上例中创建的两个任务几乎完全相同，唯一的区别就是打印输出的字符串。


```
void vTaskFunction( void *pvParameters )
{
    char *pcTaskName;
    volatile unsigned long ul;
    /* 需要打印输出的字符串从入口参数传入。强制转换为字符指针。 */
    pcTaskName = ( char * ) pvParameters;
    /* As per most tasks, this task is implemented in an infinite loop. */
    for( ;; )
    {
        /* Print out the name of this task. */
        vPrintString( pcTaskName );
        /* Delay for a period. */
        for( ul = 0; ul < mainDELAY_LOOP_COUNT; ul++ )
        {
            /* This loop is just a very crude delay implementation. There is
            nothing to do in here. Later exercises will replace this crude
            loop with a proper delay/sleep function. */
        }
    }
}

/* 定义将要通过任务参数传递的字符串。定义为const，且不是在栈空间上，以保证任务执行时也有效。 */
static const char *pcTextForTask1 = “Task 1 is running\r\n”;
static const char *pcTextForTask2 = “Task 2 is running\t\n”;
int main( void )
{
    /* Create one of the two tasks. */
    xTaskCreate( vTaskFunction, /* 指向任务函数的指针. */
                "Task 1", /* 任务名. */
                1000, /* 栈深度. */
                (void*)pcTextForTask1, /* 通过任务参数传入需要打印输出的文本. */
                1, /* 此任务运行在优先级1上. */
                NULL ); /* 不会用到此任务的句柄. */
    /* 同样的方法创建另一个任务。至此，由相同的任务代码(vTaskFunction)创建了多个任务，仅仅是传入的参数不同。同一个任务创建了两个实例。 */
    xTaskCreate( vTaskFunction, "Task 2", 1000, (void*)pcTextForTask2, 1, NULL );
    /* Start the scheduler so our tasks start executing. */
    vTaskStartScheduler();
    /* If all is well then main() will never reach here as the scheduler will
    now be running the tasks. If main() does reach here then it is likely that
    there was insufficient heap memory available for the idle task to be created.
    CHAPTER 5 provides more information on memory management. */
    for( ;; );
}
```

## 任务优先级

xTaskCreate() API 函数的参数 uxPriority 为创建的任务赋予了一个初始优先级。这个侁先级可以在调度器启动后调用 ==vTaskPrioritySet()== API 函数进行修改。

在 文 件 ==FreeRTOSConfig.h== 中 设 定 的 编 译 时 配 置 常 量==configMAX_PRIORITIES== 的值，即是最多可具有的优先级数目。

低优先级号表示任务的优先级低，优先级号 0 表示最低优先级。有效的优先级号范围从 0 到(configMAX_PRIORITES – 1)。

#### 调度器基本工作原理

- 调度器保证总是在所有可运行的任务中选择具有最高优先级的任务，并使其进入运行态。如果被选中的优先级上具有不止一个任务，调度器会让这些任务轮流执行。（任务在时间片起始时刻进入运行态，在时间片结束时刻又退出运行态。）

- 要能够选择下一个运行的任务，调度器需要在每个时间片的结束时刻运行自己本身。一个称为心跳(tick，有些地方被称为时钟滴答，本文中一律称为时钟心跳)中断的周期性中断用于此目的。==时间片的长度通过心跳中断的频率进行设定==，心跳中断频率由FreeRTOSConfig.h 中的编译时配置常量 ==configTICK_RATE_HZ== 进行配置。比如说，如果 configTICK_RATE_HZ 设为 100(HZ)，则时间片长度为 10ms。
- 简略程序执行流程

    ![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCEb35606ba2054fc69ef69db60e178f098/6247)

    注意：如果高优先级任务不释放资源，低优先级任务是没办法执行任务的。所以需要引入“非运行态”。
    
#### 改变任务优先级
API 函数 vTaskPriofitySet()可以用于在调度器启动后改变任何任务的优先级。

```
void vTaskPrioritySet( xTaskHandle pxTask,   //被修改优先级的任务句柄(即目标任务),任务可以通过传入 NULL 值来修改自己的优先级。
                    unsigned portBASE_TYPE uxNewPriority ); //目标任务将被设置到哪个优先级上。
```

uxTaskPriorityGet() API 函数用于查询一个任务的优先级。

```
unsigned portBASE_TYPE uxTaskPriorityGet( xTaskHandle pxTask );//被查询任务的句柄(目标任务) ,任务可以通过传入 NULL 值来查询自己的优先级.
```

#### 改变任务优先级示例

```
void vTask1( void *pvParameters )
{
    unsigned portBASE_TYPE uxPriority;
    /* 本任务将会比任务2更先运行，因为本任务创建在更高的优先级上。任务1和任务2都不会阻塞，所以两者要么处于就绪态，要么处于运行态。查询本任务当前运行的优先级 – 传递一个NULL值表示说“返回我自己的优先级”。 */
    uxPriority = uxTaskPriorityGet( NULL );
    for( ;; )
    {
        /* Print out the name of this task. */
        vPrintString( "Task1 is running\r\n" );
        /* 把任务2的优先级设置到高于任务1的优先级，会使得任务2立即得到执行(因为任务2现在是所有任务中具有最高优先级的任务)。注意调用vTaskPrioritySet()时用到的任务2的句柄。程序清单24将展示如何得到这个句柄。 */
        vPrintString( "About to raise the Task2 priority\r\n" );
        vTaskPrioritySet( xTask2Handle, ( uxPriority + 1 ) );
        /* 本任务只会在其优先级高于任务2时才会得到执行。因此，当此任务运行到这里时，
        任务2必然已经执行过了，并且将其自身的优先级设置回比任务1更低的优先级。 */
    }
}

void vTask2( void *pvParameters )
{
    unsigned portBASE_TYPE uxPriority;
    /* 任务1比本任务更先启动，因为任务1创建在更高的优先级。任务1和任务2都不会阻塞，
    所以两者要么处于就绪态，要么处于运行态。查询本任务当前运行的优先级 – 传递一个NULL值表示说“返回我自己的优先级”。 */
    uxPriority = uxTaskPriorityGet( NULL );
    for( ;; )
    {
        /* 当任务运行到这里，任务1必然已经运行过了，并将本身务的优先级设置到高于任务1本身。 */
        vPrintString( "Task2 is running\r\n" );
        /* 将自己的优先级设置回原来的值。传递NULL句柄值意味“改变我己自的优先级”。
        把优先级设置到低于任务1使得任务1立即得到执行 – 任务1抢占本任务。 */
        vPrintString( "About to lower the Task2 priority\r\n" );
        vTaskPrioritySet( NULL, ( uxPriority - 2 ) );
    }
}

/* 声明变量用于保存任务2的句柄。 */
xTaskHandle xTask2Handle;

int main( void )
{
    /* 任务1创建在优先级2上。任务参数没有用到，设为NULL。任务句柄也不会用到，也设为NULL */
    xTaskCreate( vTask1, "Task 1", 1000, NULL, 2, NULL );
    /* The task is created at priority 2 ______^. */
    /* 任务2创建在优先级1上 – 此优先级低于任务1。任务参数没有用到，设为NULL。但任务2的任务句柄会被
    用到，故将xTask2Handle的地址传入。 */
    xTaskCreate( vTask2, "Task 2", 1000, NULL, 1, &xTask2Handle );
    /* The task handle is the last parameter _____^^^^^^^^^^^^^ */
    /* Start the scheduler so the tasks start executing. */
    vTaskStartScheduler();
    /* If all is well then main() will never reach here as the scheduler will
    now be running the tasks. If main() does reach here then it is likely that
    there was insufficient heap memory available for the idle task to be created.
    CHAPTER 5 provides more information on memory management. */
    for( ;; );
}

```


    
## 非运行态

为了使我们的任务切实有用，我们需要通过某种方式来进行事件驱动。一个事件驱动任务只会在事件发生后触发工作(处理)，而在事件没有发生时是不能进入运行态的。调度器总是选择所有能够进入运行态的任务中具有最高优先级的任务。一个高优先级但不能够运行的任务意味着不会被调度器选中，而代之以另一个优先级虽然更低但能够运行的任务。因此，采用事件驱动任务的意义就在于任务可以被创建在许多不同的优先级上，并且最高优先级任务不会把所有的低优先级任务饿死。


#### 阻塞状态

如果一个任务正在等待某个事件，则称这个任务处于”阻塞态(blocked)”。阻塞态是非运行态的一个子状态

任务可以进入阻塞态以等待以下两种不同类型的事件：
1. 定时(时间相关)事件，延迟到期或精准时间到点。比如说某个任务可以进入阻塞态以延迟 10ms。
    
    - 延迟到期
    ```
    void vTaskDelay( portTickType xTicksToDelay );
    //xTicksToDelay:延迟多少个心跳周期。调用该延迟函数的任务将进入阻塞态，经延迟指定的心跳周期数后，再转移到就绪态。
    //常量portTICK_RATE_MS可以用来在毫秒和心跳周期之间相换转换。如：vTaskDelay( 250/portTICK_RATE_MS );
    ```
    - 精准时间到期
    
    vTaskDelayUntil()的参数就是用来指定任务离开阻塞态进入就绪态那一刻的精确心跳计数值。可以用于实现一个固定执行周期的需求.
    ```
    void vTaskDelayUntil( portTickType * pxPreviousWakeTime, portTickType xTimeIncrement );
    //pxPreviousWakeTime保存了任务上一次离开阻塞态(被唤醒)的时刻。这个时刻被用作一个参考点来计算该任务下一次离开阻塞态的时刻。
    //pxPreviousWakeTime 指 向 的 变 量 值 会 在 API 函数vTaskDelayUntil()调用过程中自动更新，应用程序除了该变量第一次初始化外，通常都不要修改它的值。
    
    //xTimeIncrement: 指定任务固定周期执行的频率
    ```

    

2. 同步事件，源于其它任务或中断的事件。
    例如：
    1. FreeRTOS 的队列
    2. 二值信号量
    3. 计数信号量
    4. 互斥信号量
    5. 互斥量

#### 挂起状态

“挂起(suspended)”也是非运行状态的子状态。处于挂起状态的任务对调度器而言是不可见的。

让一个任务进入挂起状态的唯一办法就是调用 ==vTaskSuspend()== API 函数；而 把 一 个 挂 起 状 态 的 任 务 唤 醒 的 唯 一 途 径 就 是 调 用 ==vTaskResume()== 或==vTaskResumeFromISR()== API 函数。

#### 就绪状态

如果任务处于非运行状态，但既没有阻塞也没有挂起，则这个任务处于就绪(ready，准备或就绪)状态。处于就绪态的任务能够被运行。

#### 完整的状态转移图

![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCE5bfd83e731a77804107bea1aee3e2502/6299)

#### 延时任务示例

```
void vTaskFunction( void *pvParameters )
{
    char *pcTaskName;
    /* The string to print out is passed in via the parameter. Cast this to a
    character pointer. */
    pcTaskName = ( char * ) pvParameters;
    /* As per most tasks, this task is implemented in an infinite loop. */
    for( ;; )
    {
        /* Print out the name of this task. */
        vPrintString( pcTaskName );
        /* 延迟一个循环周期。 调用vTaskDelay()以让任务在延迟期间保持在阻塞态。延迟时间以心跳周期为单位，常量portTICK_RATE_MS可以用来在毫秒和心跳周期之间相换转换。本例设定250毫秒的循环周期。 */
        vTaskDelay( 250 / portTICK_RATE_MS );
    }
}
```

#### 精准周期任务示例

```
void vTaskFunction( void *pvParameters )
{
    char *pcTaskName;
    portTickType xLastWakeTime;
    /* The string to print out is passed in via the parameter. Cast this to a
    character pointer. */
    pcTaskName = ( char * ) pvParameters;
    /* 变量xLastWakeTime需要被初始化为当前心跳计数值。说明一下，这是该变量唯一一次被显式赋值。之后，xLastWakeTime将在函数vTaskDelayUntil()中自动更新。 */
    xLastWakeTime = xTaskGetTickCount();
    /* As per most tasks, this task is implemented in an infinite loop. */
    for( ;; )
    {
        /* Print out the name of this task. */
        vPrintString( pcTaskName );
        /* 本任务将精确的以250毫秒为周期执行。同vTaskDelay()函数一样，时间值是以心跳周期为单位的，可以使用常量portTICK_RATE_MS将毫秒转换为心跳周期。变量xLastWakeTime会在vTaskDelayUntil()中自动更新，因此不需要应用程序进行显示更新。 */
        vTaskDelayUntil( &xLastWakeTime, ( 250 / portTICK_RATE_MS ) );
    }
}
```


## 空闲任务与钩子函数

当调用 vTaskStartScheduler()时，调度器会自动创建一个空闲任务。空闲任务是一个非常短小的循环。空闲任务拥有最低优先级(优先级 0)以保证其不会妨碍具有更高优先级的应用任务进入运行态。

#### 空闲任务钩子函数
通过空闲任务钩子函数(或称回调， hook, or call-back)，可以直接在空闲任务中添加应用程序相关的功能。==空闲任务钩子函数会被空闲任务每循环一次就自动调用一次==。

```
//函数原型
void vApplicationIdleHook( void );
```

##### 应用范围
- 执行低优先级，后台或需要不停处理的功能代码。
- 测试系统处理裕量。
- 将处理器配置到低功耗模式——提供一种自动省电方法。

##### 实现限制
- 绝不能阻塞或挂起。
- 如果应用程序用到了 vTaskDelete() AP 函数，则空闲钩子函数必须能够尽快返回。让空闲任务负责回收内核资源。
 
#### 空闲任务钩子函数使用示例

FreeRTOSConfig.h 中的配置常量 configUSE_IDLE_HOOK 必须定义为 1，这样空闲任务钩子函数才会被调用。


```
对应用任务实现函数进行了少量的修改，用以打印输出变量 ulIdleCycleCount 的值。

/* Declare a variable that will be incremented by the hook function. */
unsigned long ulIdleCycleCount = 0UL;
/* 空闲钩子函数必须命名为vApplicationIdleHook(),无参数也无返回值。 */
void vApplicationIdleHook( void )
{
    /* This hook function does nothing but increment a counter. */
    ulIdleCycleCount++;
}

void vTaskFunction( void *pvParameters )
{
    char *pcTaskName;
    /* The string to print out is passed in via the parameter. Cast this to a
    character pointer. */
    pcTaskName = ( char * ) pvParameters;
    /* As per most tasks, this task is implemented in an infinite loop. */
    for( ;; )
    {
        /* 打印输出任务名，以及调用计数器ulIdleCycleCount的值。 */
        vPrintStringAndNumber( pcTaskName, ulIdleCycleCount );
        /* Delay for a period for 250 milliseconds. */
        vTaskDelay( 250 / portTICK_RATE_MS );
    }
}

//钩子函数相当于空闲任务一个子函数，因此在mcu空闲时期，钩子函数计数会一直增加。

```

## 删除任务

任务可以使用 API 函数 vTaskDelete()删除自己或其它任务。

空闲任务的责任是要将分配给已删除任务的内存释放掉。因此有一点很重要，那就
是使用 vTaskDelete() API 函数的任务千万不能把空闲任务的执行时间饿死。
（只有内核为任务分配的内存空间才会在任务被删除后自动回收。
任务自己占用的内存或资源需要由应用程序自己显式地释放。）


```
void vTaskDelete( xTaskHandle pxTaskToDelete ); //被删除任务的句柄(目标任务),任务可以通过传入 NULL 值来删除自己。
```
#### 删除任务示例

```
void vTask1( void *pvParameters )
{
    const portTickType xDelay100ms = 100 / portTICK_RATE_MS;
    for( ;; )
    {
        /* Print out the name of this task. */
        vPrintString( "Task1 is running\r\n" );
        /* 创建任务2为最高优先级。 */
        xTaskCreate( vTask2, "Task 2", 1000, NULL, 2, &xTask2Handle );
        /* The task handle is the last parameter _____^^^^^^^^^^^^^ */
        /* 因为任务2具有最高优先级，所以任务1运行到这里时，任务2已经完成执行，删除了自己。任务1得以执行，延迟100ms */
        vTaskDelay( xDelay100ms );
    }
}

void vTask2( void *pvParameters )
{
    /* 任务2什么也没做，只是删除自己。删除自己可以传入NULL值，这里为了演示，还是传入其自己的句柄。 */
    vPrintString( "Task2 is running and about to delete itself\r\n" );
    vTaskDelete( xTask2Handle );
}

int main( void )
{
    /* 任务1创建在优先级1上 */
    xTaskCreate( vTask1, "Task 1", 1000, NULL, 1, NULL );
    /* The task is created at priority 1 ______^. */
    /* Start the scheduler so the tasks start executing. */
    vTaskStartScheduler();
    /* main() should never reach here as the scheduler has been started. */
    for( ;; );
}

```

## 调度算法

### 优先级抢占式调度
- 每个任务都赋予了一个优先级。
- 每个任务都可以存在于一个或多个状态。
- 在任何时候都只有一个任务可以处于运行状态。
- 调度器总是在所有处于就绪态的任务中选择具有最高优先级的任务来执行。

这种类型的调度方案被称为”固定优先级抢占式调度”。每个任务都被赋予了一个优先级，这个优先级不能被内核本身改变(只能被任务修改)。

任务可以在阻塞状态等待一个事件，当事件发生时其将自动回到就绪态。时间事件
发生在某个特定的时刻，比如阻塞超时。时间事件通常用于周期性或超时行为。任务或
中断服务例程往队列发送消息或发送任务一种信号量，都将触发同步事件。同步事件通
常用于触发同步行为，比如某个外围的数据到达了。


### 协作式调度
采用一个纯粹的协作式调度器，只可能在运行态任务进入阻塞态或是运行态任务显
式调用 taskYIELD()时，才会进行上下文切换。任务永远不会被抢占，而具有相同优先
级的任务也不会自动共享处理器时间。协作式调度的这作工作方式虽然比较简单，但可
能会导致系统响应不够快。




---

# 第二章 队列管理

基于 FreeRTOS 的应用程序由一组独立的任务构成——每个任务都是具有独立权
限的小程序。这些独立的任务之间很可能会通过相互通信以提供有用的系统功能。
FreeRTOS 中所有的通信与同步机制都是基于==队列==实现的。

## 本章目的
- 如何创建一个队列
- 队列如何管理其数据
- 如何向队列发送数据
- 如何从队列接收数据
- 队列阻塞是什么意思
- 往队列发送和从队列接收时，任务优先级会有什么样的影响

## 队列的特性
- 数据存储

    队列可以保存有限个具有确定长度的数据单元。队列被作为 FIFO(先进先出)使用。
- 可被多任务存取

    队列是具有自己独立权限的内核对象，并不属于或赋予任何任务。所有任务都可以向同一队列写入和读出。
- 读队列时阻塞

    当某个任务试图读一个队列时，其可以指定一个阻塞超时时间。在这段时间中，如
果队列为空，该任务将保持阻塞状态以等待队列数据有效。当其它任务或中断服务例程
往其等待的队列中写入了数据，该任务将自动由阻塞态转移为就绪态。当等待的时间超
过了指定的阻塞时间，即使队列中尚无有效数据，任务也会自动从阻塞态转移为就绪态。
    
    由于队列可以被多个任务读取，所以对单个队列而言，也可能有多个任务处于阻塞
状态以等待队列数据有效。这种情况下，一旦队列数据有效，只会有一个任务会被解除
阻塞，这个任务就是所有等待任务中优先级最高的任务。而如果所有等待任务的优先级
相同，那么被解除阻塞的任务将是等待最久的任务。

- 写队列时阻塞

    同读队列一样，任务也可以在写队列时指定一个阻塞超时时间。这个时间是当被写
队列已满时，任务进入阻塞态以等待队列空间有效的最长时间。

    由于队列可以被多个任务写入，所以对单个队列而言，也可能有多个任务处于阻塞
状态以等待队列空间有效。这种情况下，一旦队列空间有效，只会有一个任务会被解除
阻塞，这个任务就是所有等待任务中优先级最高的任务。而如果所有等待任务的优先级
相同，那么被解除阻塞的任务将是等待最久的任务。

- 队列读写过程

    ![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCE51b2ad094f6dbdedeac68c02423a6c6d/6515)
    
## 使用队列


#### xQueueCreate() API 函数
xQueueCreate()用于创建一个队列，并返回一个 xQueueHandle 句柄以便于对其创建的队列进行引用。
    
当创建队列时， FreeRTOS 从堆空间中分配内存空间。分配的空间用于存储队列数
据结构本身以及队列中包含的数据单元。如果内存堆中没有足够的空间来创建队列，
xQueueCreate()将返回 NULL。

```
xQueueHandle xQueueCreate( unsigned portBASE_TYPE uxQueueLength,  //队列能够存储的最大单元数目，即队列深度。
                        unsigned portBASE_TYPE uxItemSize );  //队列中数据单元的长度，以字节为单位。
                        
                        //NULL 表示没有足够的堆空间分配给队列而导致创建失败。
                       //非NULL值表示队列创建成功。此返回值应当保存下来，以作为操作此队列的句柄。
```

#### xQueueSendToBack() 与 xQueueSendToFront() API 函数
xQueueSendToBack()用于将数据发送到队列尾；而 xQueueSendToFront()用于将数据发送到队列首。

```
portBASE_TYPE xQueueSendToFront( xQueueHandle xQueue,   //目标队列的句柄
                    const void * pvItemToQueue,         //发送数据的指针
                    portTickType xTicksToWait );    //阻塞超时时间,以系统心跳周期为单位
//有两个可能的返回值:
1. pdPASS, 数据被成功发送到队列
2. errQUEUE_FULL,   队列已满而无法将数据写入

portBASE_TYPE xQueueSendToBack( xQueueHandle xQueue,    //目标队列的句柄
                    const void * pvItemToQueue,         //发送数据的指针
                    portTickType xTicksToWait );    //阻塞超时时间,以系统心跳周期为单位
//有两个可能的返回值:
1. pdPASS, 数据被成功发送到队列
2. errQUEUE_FULL,   队列已满而无法将数据写入

```
#### xQueueReceive()与 xQueuePeek() API 函数
xQueueReceive()用于从队列中接收(读取）数据单元。接收到的单元同时会从队列中删除.

xQueuePeek()也是从从队列中接收数据单元，不同的是并不从队列中删出接收到的单元。

```
portBASE_TYPE xQueueReceive( xQueueHandle xQueue,  //被读队列的句柄
                        const void * pvBuffer,      //接收缓存指针
                        portTickType xTicksToWait );//阻塞超时时间。

portBASE_TYPE xQueuePeek( xQueueHandle xQueue,      //被读队列的句柄
                        const void * pvBuffer,      //接收缓存指针
                        portTickType xTicksToWait );//阻塞超时时间。
                        
```

#### uxQueueMessagesWaiting() API 函数
uxQueueMessagesWaiting()用于查询队列中当前有效数据单元个数。

切记不要在中断服务例程中调用 uxQueueMessagesWaiting()。应当在中断服务中
使用其中断安全版本 uxQueueMessagesWaitingFromISR()。

```
unsigned portBASE_TYPE uxQueueMessagesWaiting( xQueueHandle xQueue ); //被查询队列的句柄。
//返回值 当前队列中保存的数据单元个数。返回 0 表明队列为空。
```

#### 读队列时阻塞示例

本例示范创建一个队列，由多个任务往队列中写数据，以及从队列中把数据读出。
这个队列创建出来保存 long 型数据单元。往队列中写数据的任务没有设定阻塞超时时
间，而读队列的任务设定了超时时间。

往队列中写数据的任务的优先级低于读队列任务的优先级。这意味着队列中永远不
会保持超过一个的数据单元。因为一旦有数据被写入队列，读队列任务立即解除阻塞，
抢占写队列任务，并从队列中接收数据，同时数据从队列中删除—队列再一次变为空队列。

```
static void vSenderTask( void *pvParameters )
{
    long lValueToSend;
    portBASE_TYPE xStatus;
    /* 该任务会被创建两个实例，所以写入队列的值通过任务入口参数传递 – 这种方式使得每个实例使用不同的
    值。队列创建时指定其数据单元为long型，所以把入口参数强制转换为数据单元要求的类型 */
    lValueToSend = ( long ) pvParameters;
    /* 和大多数任务一样，本任务也处于一个死循环中 */
    for( ;; )
    {
        /* 往队列发送数据
        第一个参数是要写入的队列。队列在调度器启动之前就被创建了，所以先于此任务执行。
        第二个参数是被发送数据的地址，本例中即变量lValueToSend的地址。
        第三个参数是阻塞超时时间 – 当队列满时，任务转入阻塞状态以等待队列空间有效。本例中没有设定超
        时时间，因为此队列决不会保持有超过一个数据单元的机会，所以也决不会满。
        */
        xStatus = xQueueSendToBack( xQueue, &lValueToSend, 0 );
        if( xStatus != pdPASS )
        {
            /* 发送操作由于队列满而无法完成 – 这必然存在错误，因为本例中的队列不可能满。*/
            vPrintString( "Could not send to the queue.\r\n" );
        }
        /* 允许其它发送任务执行。 taskYIELD()通知调度器现在就切换到其它任务，而不必等到本任务的时间片耗尽 */
        taskYIELD();
    }
}

static void vReceiverTask( void *pvParameters )
{
    /* 声明变量，用于保存从队列中接收到的数据。 */
    long lReceivedValue;
    portBASE_TYPE xStatus;
    const portTickType xTicksToWait = 100 / portTICK_RATE_MS;
    /* 本任务依然处于死循环中。 */
    for( ;; )
    {
        /* 此调用会发现队列一直为空，因为本任务将立即删除刚写入队列的数据单元。 */
        if( uxQueueMessagesWaiting( xQueue ) != 0 )
        {
            vPrintString( "Queue should have been empty!\r\n" );
        }
        /* 从队列中接收数据
        第一个参数是被读取的队列。队列在调度器启动之前就被创建了，所以先于此任务执行。
        第二个参数是保存接收到的数据的缓冲区地址，本例中即变量lReceivedValue的地址。此变量类型与
        队列数据单元类型相同，所以有足够的大小来存储接收到的数据。
        第三个参数是阻塞超时时间 – 当队列空时，任务转入阻塞状态以等待队列数据有效。本例中常量
        portTICK_RATE_MS用来将100毫秒绝对时间转换为以系统心跳为单位的时间值。
        */
        xStatus = xQueueReceive( xQueue, &lReceivedValue, xTicksToWait );
        if( xStatus == pdPASS )
        {
            /* 成功读出数据，打印出来。 */
            vPrintStringAndNumber( "Received = ", lReceivedValue );
        }
        else
        {
            /* 等待100ms也没有收到任何数据。
            必然存在错误，因为发送任务在不停地往队列中写入数据 */
            vPrintString( "Could not receive from the queue.\r\n" );
        }
    }
}

/* 声明一个类型为 xQueueHandle 的变量. 其用于保存队列句柄，以便三个任务都可以引用此队列 */
xQueueHandle xQueue;
int main( void )
{
    /* 创建的队列用于保存最多5个值，每个数据单元都有足够的空间来存储一个long型变量 */
    xQueue = xQueueCreate( 5, sizeof( long ) );
    if( xQueue != NULL )
    {
        /* 创建两个写队列任务实例，任务入口参数用于传递发送到队列的值。所以一个实例不停地往队列发送
        100，而另一个任务实例不停地往队列发送200。两个任务的优先级都设为1。 */
        xTaskCreate( vSenderTask, "Sender1", 1000, ( void * ) 100, 1, NULL );
        xTaskCreate( vSenderTask, "Sender2", 1000, ( void * ) 200, 1, NULL );
        /* 创建一个读队列任务实例。其优先级设为2，高于写任务优先级 */
        xTaskCreate( vReceiverTask, "Receiver", 1000, NULL, 2, NULL );
        /* 启动调度器，任务开始执行 */
        vTaskStartScheduler();
    }
    else
    {
        /* 队列创建失败*/
    }
    /* 如果一切正常， main()函数不应该会执行到这里。但如果执行到这里，很可能是内存堆空间不足导致空闲
    任务无法创建。第五章有讲述更多关于内存管理方面的信息 */
    for( ;; );
}

```

#### 写入队列时阻塞示例

只是写队列任务与读队列任务的优先级交换了，即读队列任
务的优先级低于写队列任务的优先级。并且本例中的队列用于在任务间传递结构体数
据，而非简单的长整型数据。

写队列任务具有最高优先级，所以队列正常情况下一直
是处于满状态。这是因为一旦读队列任务从队列中读走一个数据单元，某个写队列任务
就会立即抢占读队列任务，把刚刚读走的位置重新写入，之后便又转入阻塞态以等待队
列空间有效。


```
/* 定义队列传递的结构类型。 */
typedef struct
{
    unsigned char ucValue;
    unsigned char ucSource;
} xData;

/* 声明两个xData类型的变量，通过队列进行传递。 */
static const xData xStructsToSend[ 2 ] =
{
    { 100, mainSENDER_1 }, /* Used by Sender1. */
    { 200, mainSENDER_2 } /* Used by Sender2. */
};


static void vSenderTask( void *pvParameters )
{
    portBASE_TYPE xStatus;
    const portTickType xTicksToWait = 100 / portTICK_RATE_MS;
    /* As per most tasks, this task is implemented within an infinite loop. */
    for( ;; )
    {
        /* Send to the queue.
        第二个参数是将要发送的数据结构地址。这个地址是从任务入口参数中传入，所以直接使用pvParameters.
        第三个参数是阻塞超时时间 – 当队列满时，任务转入阻塞态等待队列空间有效的最长时间。指定超时时
        间是因为写队列任务的优先级高于读任务的优先级。所以队列如预期一样很快写满，写队列任务就会转入
        阻塞态，此时读队列任务才会得以执行，才能从队列中把数据读走。 */
        xStatus = xQueueSendToBack( xQueue, pvParameters, xTicksToWait );
        if( xStatus != pdPASS )
        {
            /* 写队列任务无法将数据写入队列，直至100毫秒超时。
            这必然存在错误，因为只要写队列任务进入阻塞态，读队列任务就会得到执行，从而读走数据，腾
            出空间 */
            vPrintString( "Could not send to the queue.\r\n" );
        }
        /* 让其他写队列任务得到执行。 */
        taskYIELD();
    }
}

static void vReceiverTask( void *pvParameters )
{
    /* 声明结构体变量以保存从队列中读出的数据单元 */
    xData xReceivedStructure;
    portBASE_TYPE xStatus;
    /* This task is also defined within an infinite loop. */
    for( ;; )
    {
        /* 读队列任务的优先级最低，所以其只可能在写队列任务阻塞时得到执行。而写队列任务只会在队列写
        满时才会进入阻塞态，所以读队列任务执行时队列肯定已满。所以队列中数据单元的个数应当等于队列的
        深度 – 本例中队列深度为3 */
        if( uxQueueMessagesWaiting( xQueue ) != 3 )
        {
            vPrintString( "Queue should have been full!\r\n" );
        }
        /* Receive from the queue.
        第二个参数是存放接收数据的缓存空间。本例简单地采用一个具有足够空间大小的变量的地址。
        第三个参数是阻塞超时时间 – 本例不需要指定超时时间，因为读队列任会只会在队列满时才会得到执行，
        故而不会因队列空而阻塞 */
        xStatus = xQueueReceive( xQueue, &xReceivedStructure, 0 );
        if( xStatus == pdPASS )
        {
            /* 数据成功读出，打印输出数值及数据来源。 */
            if( xReceivedStructure.ucSource == mainSENDER_1 )
            {
                vPrintStringAndNumber( "From Sender 1 = ", xReceivedStructure.ucValue );
            }
            else
            {
                vPrintStringAndNumber( "From Sender 2 = ", xReceivedStructure.ucValue );
            }
        }
        else
        {
            /* 没有读到任何数据。这一定是发生了错误，因为此任务只支在队列满时才会得到执行 */
            vPrintString( "Could not receive from the queue.\r\n" );
        }
    }
}

int main( void )
{
    /* 创建队列用于保存最多3个xData类型的数据单元。 */
    xQueue = xQueueCreate( 3, sizeof( xData ) );
    if( xQueue != NULL )
    {
        /* 为写队列任务创建2个实例。 The
        任务入口参数用于传递发送到队列中的数据。因此其中一个任务往队列中一直写入
        xStructsToSend[0]，而另一个则往队列中一直写入xStructsToSend[1]。这两个任务的优先级都
        设为2，高于读队列任务的优先级 */
        xTaskCreate( vSenderTask, "Sender1", 1000, &( xStructsToSend[ 0 ] ), 2, NULL );
        xTaskCreate( vSenderTask, "Sender2", 1000, &( xStructsToSend[ 1 ] ), 2, NULL );
        /* 创建读队列任务。
        读队列任务优先级设为1，低于写队列任务的优先级。 */
        xTaskCreate( vReceiverTask, "Receiver", 1000, NULL, 1, NULL );
        /* 启动调度器，创建的任务得到执行。 */
        vTaskStartScheduler();
    }
    else
    {
        /* 创建队列失败。 */
    }
    /* 如果一切正常， main()函数不应该会执行到这里。但如果执行到这里，很可能是内存堆空间不足导致空闲
    任务无法创建。第五章将提供更多关于内存管理方面的信息 */
    for( ;; );
}

```

#### 当使用队列通过指针传递大量数据时，需注意
1. 指针指向的内存空间的所有权必须明确
    
    当任务间通过指针共享内存时，应该从根本上保证所不会有任意两个任务同时
修改共享内存中的数据，或是以其它行为方式使得共享内存数据无效或产生一致性
问题。
2. 指针指向的内存空间必须有效

    如果指针指向的内存空间是动态分配的，只应该有一个任务负责对其进行内存
释放。当这段内存空间被释放之后，就不应该有任何一个任务再访问这段空间。


---
# 第三章 中断管理

## 本章目的
- 哪些 FreeRTOS 的 API 函数可以在中断服务例程中使用。
- 延迟中断方案是处何实现的。
- 如何创建和使用二值信号量以及计数信号量。
- 二值信号量和计数信号量之间的区别。
- 何利用队列在中断服务例程中把数据传入传出。
- 一些 FreeRTOS 移植中采用的中断嵌套模型。

## 延迟中断处理

#### 采用二值信号量同步
二值信号量可以在某个特殊的中断发生时，让任务解除阻塞，相当于让任务与中断
同步。这样就可以让中断事件处理量大的工作在同步任务中完成，中断服务(ISR)
中只是快速处理少部份工作。 如此，中断处理可以说是被”推迟(deferred)”到一个”处理
(handler)”任务。

如果某个中断处理要求特别紧急，其延迟处理任务的优先级可以设为最高，以保证
延迟处理任务随时都抢占系统中的其它任务。

延迟处理任务对一个信号量进行带阻塞性质的”==take==”调用，意思是进入阻塞态以等
待事件发生。当事件发生后， ISR 对同一个信号量进行”==give==”操作，使得延迟处理任务
解除阻塞，从而事件在延迟处理任务中得到相应的处理。

#### 创建二值信号量

```
void vSemaphoreCreateBinary( xSemaphoreHandle xSemaphore ); //创建的信号量
//需要说明的是 vSemaphoreCreateBinary()在实现上是一个宏，所以信号量变量应当直接传入，而不是传址。
```

#### 获取信号量 Take
==所有类型的信号量都可以调用函数 xSemaphoreTake()来获取。==
但 xSemaphoreTake()不能在中断服务例程中调用。中断中使用xSemaphoreTakeFromISR。

```
portBASE_TYPE xSemaphoreTake( xSemaphoreHandle xSemaphore, portTickType xTicksToWait );
//xSemaphore：获取得到的信号量
//阻塞超时时间。
//返回值: 
    1. pdPASS
    2. pdFALSE
```

#### 给予信号量 Give
==所有类型的信号量都可以调用函数 xSemaphoreGive()来给与/归还。== 但 xSemaphoreGive()不能在中断服务例程中调用。中断中使用xSemaphoreGiveFromISR。
```
portBASE_TYPE xSemaphoreGiveFromISR( xSemaphoreHandle xSemaphore,  //给出的信号量
                        portBASE_TYPE *pxHigherPriorityTaskWoken ); //输出值：如 果 xSemaphoreGiveFromISR() 将 此 值 设 为pdTRUE，
                         //则在中断退出前应当进行一次上下文切换。这样才能保证中断直接返回就绪态任务中优先级最高的任务中。
```

#### 利用二值信号量对任务和中断进行同步示例
本例在中断服务例程中使用一个二值信号量让任务从阻塞态中切换出来——从效
果上等同于让任务与中断进行同步。

一个简单的周期性任务用于每隔 500 毫秒产生一个软件中断。

整个执行流程可以描述为:
1. 中断产生。
2. 中断服务例程启动，给出信号量以使延迟处理任务解除阻塞。
3. 当中断服务例程退出时，延迟处理任务得到执行。延迟处理任务做的第一件事便是
获取信号量。
4. 延迟处理任务完成中断事件处理后，试图再次获取信号量——如果此时信号量无效，
任务将切入阻塞待等待事件发生。

```
static void vPeriodicTask( void *pvParameters )
{
    for( ;; )
    {
        /* 此任务通过每500毫秒产生一个软件中断来”模拟”中断事件 */
        vTaskDelay( 500 / portTICK_RATE_MS );
        /* 产生中断，并在产生之前和之后输出信息，以便在执行结果中直观直出执行流程 */
        vPrintString( "Periodic task - About to generate an interrupt.\r\n" );
        __asm{ int 0x82 } /* 这条语句产生中断 */
        vPrintString( "Periodic task - Interrupt generated.\r\n\r\n\r\n" );
    }
}

static void vHandlerTask( void *pvParameters )
{
    /* As per most tasks, this task is implemented within an infinite loop. */
    for( ;; )
    {
        /* 使用信号量等待一个事件。信号量在调度器启动之前，也即此任务执行之前就已被创建。任务被无超
        时阻塞，所以此函数调用也只会在成功获取信号量之后才会返回。此处也没有必要检测返回值 */
        xSemaphoreTake( xBinarySemaphore, portMAX_DELAY );
        /* 程序运行到这里时，事件必然已经发生。本例的事件处理只是简单地打印输出一个信息 */
        vPrintString( "Handler task - Processing event.\r\n" );
    }
}

static void __interrupt __far vExampleInterruptHandler( void )
{
    static portBASE_TYPE xHigherPriorityTaskWoken;
    xHigherPriorityTaskWoken = pdFALSE;
    /* 'Give' the semaphore to unblock the task. */
    xSemaphoreGiveFromISR( xBinarySemaphore, &xHigherPriorityTaskWoken );
    if( xHigherPriorityTaskWoken == pdTRUE )
    {
        /* 给出信号量以使得等待此信号量的任务解除阻塞。如果解出阻塞的任务的优先级高于当前任务的优先
        级 – 强制进行一次任务切换，以确保中断直接返回到解出阻塞的任务(优选级更高)。
        说明：在实际使用中， ISR中强制上下文切换的宏依赖于具体移植。此处调用的是基于Open Watcom DOS
        移植的宏。其它平台下的移植可能有不同的语法要求。对于实际使用的平台，请参如数对应移植自带的示
        例程序，以决定正确的语法和符号。
        */
        portSWITCH_CONTEXT();
    }
}

int main( void )
{
    /* 信号量在使用前都必须先创建。本例中创建了一个二值信号量 */
    vSemaphoreCreateBinary( xBinarySemaphore );
    /* 安装中断服务例程 */
    _dos_setvect( 0x82, vExampleInterruptHandler );
    /* 检查信号量是否成功创建 */
    if( xBinarySemaphore != NULL )
    {
        /* 创建延迟处理任务。此任务将与中断同步。延迟处理任务在创建时使用了一个较高的优先级，以保证
        中断退出后会被立即执行。在本例中，为延迟处理任务赋予优先级3 */
        xTaskCreate( vHandlerTask, "Handler", 1000, NULL, 3, NULL );
        /* 创建一个任务用于周期性产生软件中断。此任务的优先级低于延迟处理任务。每当延迟处理任务切出
        阻塞态，就会抢占周期任务*/
        xTaskCreate( vPeriodicTask, "Periodic", 1000, NULL, 1, NULL );
        /* Start the scheduler so the created tasks start executing. */
        vTaskStartScheduler();
    }
    /* 如果一切正常， main()函数不会执行到这里，因为调度器已经开始运行任务。但如果程序运行到了这里，
    很可能是由于系统内存不足而无法创建空闲任务。第五章会提供更多关于内存管理的信息 */
    for( ;; );
}

```

## 计数信号量
==一个==二值信号量最多只可以锁存一个中断事件。在锁存的事
件还未被处理之前，如果还有中断事件发生，那么后续发生的中断事件将会丢失。如果
用计数信号量代替二值信号量，那么，这种丢中断的情形将可以避免。
（一个二值信号量最多只能锁存一个中断事件）

计数信号量可以看作是深度大于1的队列。计数信号量每次被给出(Given)，
其队列中的另一个空间将会被使用。队列中的有效数据单元个数就是信号量的”计数(Count)”值。

#### 计数信号量有以下两种典型用法
1. 事件计数
    
    每次事件发生时，中断服务例程都会“给出(Give)”信号量——信号
量在每次被给出时其计数值加 1。延迟处理任务每处理一个任务都会”获取(Take)”一次
信号量——信号量在每次被获取时其计数值减 1。信号量的计数值其实就是已发生事件
的数目与已处理事件的数目之间的差值。

2. 资源管理
    
    信号量的计数值用于表示可用资源的数目。一个任务要获取资源的
控制权，其必须先获得信号量——使信号量的计数值减 1。当计数值减至 0，则表示没
有可用资源。当任务利用资源完成工作后，将给出(归还)信号量——使信号量的计数值
加 1。

#### 创建计数信号量

```
xSemaphoreHandle xSemaphoreCreateCounting( unsigned portBASE_TYPE uxMaxCount,  //最大计数值
                                        unsigned portBASE_TYPE uxInitialCount );  //信号量的初始计数值
//如果返回非 NULL 值，则表示信号量创建成功。此值应当被保存起来作为这个的信号量的句柄。
                                        
```

####  利用计数信号量对任务和中断进行同步示例

```
xCountingSemaphore = xSemaphoreCreateCounting( 10, 0 );

static void __interrupt __far vExampleInterruptHandler( void )
{
    static portBASE_TYPE xHigherPriorityTaskWoken;
    xHigherPriorityTaskWoken = pdFALSE;
    /* 多次给出信号量。第一次给出时使得延迟处理任务解除阻塞。后续给出用于演示利用被信号量锁存事件，
    以便延迟处理任何依序对这些中断事件进行处理而不会丢中断。用这种方式来模拟处理器产生多个中断，尽管
    这些事件只是在单次中断中模拟出来的 */
    xSemaphoreGiveFromISR( xCountingSemaphore, &xHigherPriorityTaskWoken );
    xSemaphoreGiveFromISR( xCountingSemaphore, &xHigherPriorityTaskWoken );
    xSemaphoreGiveFromISR( xCountingSemaphore, &xHigherPriorityTaskWoken );
    if( xHigherPriorityTaskWoken == pdTRUE )
    {
        /* 给出信号量以使得等待此信号量的任务解除阻塞。如果解出阻塞的任务的优先级高于当前任务的优先
        级 – 强制进行一次任务切换，以确保中断直接返回到解出阻塞的任务(优选级更高)。
        说明：在实际使用中， ISR中强制上下文切换的宏依赖于具体移植。此处调用的是基于Open Watcom DOS
        移植的宏。其它平台下的移植可能有不同的语法要求。对于实际使用的平台，请参如数对应移植自带的示
        例程序，以决定正确的语法和符号。
        */
        portSWITCH_CONTEXT();
    }
}

//每次中断发生后，延迟处理任务处理了中断生成的全部三个事件
```

## 在中断服务中使用队列
xQueueSendToFrontFromISR()， xQueueSendToBackFromISR()与 xQueueReceiveFromISR()
分别是 xQueueSendToFront()， xQueueSendToBack()与 xQueueReceive()的中断安全
版本，专门用于中断服务例程中。


```
portBASE_TYPE xQueueSendToFrontFromISR( xQueueHandle xQueue, //目标队列的句柄
                                void *pvItemToQueue  //发送数据的指针
                                portBASE_TYPE *pxHigherPriorityTaskWoken );  //返回优先级关系

portBASE_TYPE xQueueSendToBackFromISR( xQueueHandle xQueue,
                                void *pvItemToQueue
                                portBASE_TYPE *pxHigherPriorityTaskWoken
);
```

#### 有效使用队列
- 将接收到的字符先缓存到内存中。当接收到一个传输完成消息，或是检测到传输中断后，使用信号量让某个任务解除阻塞，这个任务将对字符缓存进行处理。
- 在中断服务中直接解析接收到的字符，然后通过队列将解析后经解码得到的命令发
送到处理任务

#### 利用队列在中断服务中发送或接收数据示例

```
static void vIntegerGenerator( void *pvParameters )
{
    portTickType xLastExecutionTime;
    unsigned portLONG ulValueToSend = 0;
    int i;
    /* 初始化变量，用于调用 vTaskDelayUntil(). */
    xLastExecutionTime = xTaskGetTickCount();
    for( ;; )
    {
        /* 这是个周期性任务。进入阻塞态，直到该再次运行的时刻。此任务每200毫秒执行一次 */
        vTaskDelayUntil( &xLastExecutionTime, 200 / portTICK_RATE_MS );
        /* 连续五次发送递增数值到队列。这此数值将在中断服务例程中读出。中断服务例程会将队列读空，所
        以此任务可以确保将所有的数值都发送到队列。因此不需要指定阻塞超时时间 */
        for( i = 0; i < 5; i++ )
        {
            xQueueSendToBack( xIntegerQueue, &ulValueToSend, 0 );
            ulValueToSend++;
        }
        /* 产生中断，以让中断服务例程读取队列 */
        vPrintString( "Generator task - About to generate an interrupt.\r\n" );
        __asm{ int 0x82 } /* This line generates the interrupt. */
        vPrintString( "Generator task - Interrupt generated.\r\n\r\n\r\n" );
    }
}

static void __interrupt __far vExampleInterruptHandler( void )
{
    static portBASE_TYPE xHigherPriorityTaskWoken;
    static unsigned long ulReceivedNumber;
    /* 这些字符串被声明为static const，以保证它们不会被定位到ISR的栈空间中，即使ISR没有运行它们也是存
    在的 */
    static const char *pcStrings[] =
    {
        "String 0\r\n",
        "String 1\r\n",
        "String 2\r\n",
        "String 3\r\n"
    };
    xHigherPriorityTaskWoken = pdFALSE;
    /* 重复执行，直到队列为空 */
    while( xQueueReceiveFromISR( xIntegerQueue,&ulReceivedNumber,&xHigherPriorityTaskWoken ) != errQUEUE_EMPTY )
    {
        /* 截断收到的数据，保留低两位(数值范围0到3).然后将索引到的字符串指针发送到另一个队列 */
        ulReceivedNumber &= 0x03;
        xQueueSendToBackFromISR( xStringQueue,
        &pcStrings[ ulReceivedNumber ],
        &xHigherPriorityTaskWoken );
    }
    /* 被队列读写操作解除阻塞的任务，其优先级是否高于当前任务？如果是，则进行任务上下文切换 */
    if( xHigherPriorityTaskWoken == pdTRUE )
    {
        /* 说明：在实际使用中， ISR中强制上下文切换的宏依赖于具体移植。此处调用的是基于Open Watcom
        DOS移植的宏。其它平台下的移植可能有不同的语法要求。对于实际使用的平台，请参如数对应移植自带
        的示例程序，以决定正确的语法和符号。 */
        portSWITCH_CONTEXT();
    }
}

static void vStringPrinter( void *pvParameters )
{
    char *pcString;
    for( ;; )
    {
        /* Block on the queue to wait for data to arrive. */
        xQueueReceive( xStringQueue, &pcString, portMAX_DELAY );
        /* Print out the string received. */
        vPrintString( pcString );
    }
}

int main( void )
{
    /* 队列使用前必须先创建。本例中创建了两个队列。一个队列用于保存类型为unsigned long的变量，另一
    个队列用于保存类型为char*的变量。两个队列的深度都为10。在实际应用中应当检测返回值以确保队列创建
    成功 */
    xIntegerQueue = xQueueCreate( 10, sizeof( unsigned long ) );
    xStringQueue = xQueueCreate( 10, sizeof( char * ) );
    /* 安装中断服务例程。 */
    _dos_setvect( 0x82, vExampleInterruptHandler );
    /* 创建任务用于往中断服务例程中发送数值。此任务优先级为1 */
    xTaskCreate( vIntegerGenerator, "IntGen", 1000, NULL, 1, NULL );
    /* 创建任务用于从中断服务例程中接收字符串，并打印输出。此任务优先级为2 */
    xTaskCreate( vStringPrinter, "String", 1000, NULL, 2, NULL );
    /* Start the scheduler so the created tasks start executing. */
    vTaskStartScheduler();
    /* 如果一切正常， main()函数不会执行到这里，因为调度器已经开始运行任务。但如果程序运行到了这里，
    很可能是由于系统内存不足而无法创建空闲任务。第五章会提供更多关于内存管理的信息 */
    for( ;; );
}

```


## 中断嵌套

中断嵌套需要在 FreeRTOSConfig.h 中定义表 详细列出的一个或两个常量。

> configKERNEL_INTERRUPT_PRIORITY 

设置系统心跳时钟的中断优先级。如果在移植中没有使用常量configMAX_SYSCALL_INTERRUPT_PRIORITY，
那么需要调用中断安全版本 FreeRTOS API的中断都必须运行在此优先级上。

> configMAX_SYSCALL_INTERRUPT_PRIORITY 

设置中断安全版本 FreeRTOS API 可以运行的最高中断优先级。

建立一个全面的中断嵌套模型需要设置 configMAX_SYSCALL_INTERRUPT_PRIRITY
为比 configKERNEL_INTERRUPT_PRIORITY 更高的优先级。

- 处于中断优先级 1 到 3(含)的中断会被内核或处于临界区的应用程序阻塞执行， 但是
它们可以调用中断安全版本的 FreeRTOS API 函数。
- 处于中断优先级 4 及以上的中断不受临界区影响，所以其不会被内核的任何行为阻
塞，可以立即得到执行——这是由微控制器本身对中断优先级的限定所决定的。通
常 需 要 严 格 时 间 精 度 的 功 能 ( 如 电 机 控 制 ) 会 使 用 高 于
configMAX_SYSCALL_INTERRUPT_PRIRITY 的优先级，以保证调度器不会对其中断响
应时间造成抖动。
- 不需要调用任何 FreeRTOS API 函数的中断，可以自由地使用任意优先级。


==Cortex M3 使用低优先级号数值表示逻辑上的高优先级中断。== 比如
STM32， ST 的驱动库中将最低优先级指定为 15，而最高优先级指定为 0。



---

# 第四章 资源管理
多任务系统中存在一种潜在的风险。当一个任务在使用某个资源的过程中，即还没
有完全结束对资源的访问时，便被切出运行态，使得资源处于非一致，不完整的状态。

##### 情况
- 访问外设
- 读-改-写操作
- 变量的非原子访问
- 函数重入

## 本章目的
- 为什么，以及在什么时候有必要进行资源管理与控制。
- 什么是临界区。
- 互斥是什么意思。
- 挂起调度器有什么意义。
- 如何使用互斥量。
- 如何创建与使用守护任务。
- 什么是优先级反转，以及优先级继承是如何减小(但不是消除)其影响的。


## 互斥
访问一个被多任务共享，或是被任务与中断共享的资源时，需要采用”互斥”技术以
保证数据在任何时候都保持一致性。

最好的互斥方法（如果可能的话，
任何时候都当如此）还是通过精心设计应用程序，尽量不要共享资源，或者是每个资源
都通过单任务访问。

## 临界区与挂起调度器

#### 基本临界区
基本临界区是指宏 taskENTER_CRITICAL()与 taskEXIT_CRITICAL()之间的代码区间。

临界区是提供互斥功能的一种非常原始的实现方法。临界区的工作仅仅是简单地把
中断全部关掉，或是关掉优先级在 configMAX_SYSCAL_INTERRUPT_PRIORITY 及
以下的中断。
```
/* 为了保证对PORTA寄存器的访问不被中断，将访问操作放入临界区。
进入临界区 */
taskENTER_CRITICAL();
/* 在taskENTER_CRITICAL() 与 taskEXIT_CRITICAL()之间不会切换到其它任务。 中断可以执行，也允许
嵌套，但只是针对优先级高于configMAX_SYSCALL_INTERRUPT_PRIORITY的中断 – 而且这些中断不允许访问
FreeRTOS API 函数. */
PORTA |= 0x01;
/* 我们已经完成了对PORTA的访问，因此可以安全地离开临界区了。 */
taskEXIT_CRITICAL();
```
#### 挂起(锁定)调度器
==基本临界区==保护一段代码区间不被==其它任务或中断==打断。

==挂起调度器实现的临界区==只可以保护一段代码区间不被==其它任务==打断，因为这种方式下，中断是使能的。

```
void vTaskSuspendAll( void );
//通过调用 vTaskSuspendAll()来挂起调度器。挂起调度器可以停止上下文切换而不用关中断。
//如果某个中断在调度器挂起过程中要求进行上下文切换，则个这请求也会被挂起，直到调度器被唤醒后才会得到执行。
```
#### 唤醒调度器
```
portBASE_TYPE xTaskResumeAll( void );
//如果一个挂起的上下文切换请求在xTaskResumeAll()返回前得到执行，则函数返回 pdTRUE。
//在其它情况下， xTaskResumeAll()返回 pdFALSE。
```
#### 使用示例

```
void vPrintString( const portCHAR *pcString )
{
    /* Write the string to stdout, suspending the scheduler as a method
    of mutual exclusion. */
    vTaskSuspendScheduler();
    {
        printf( "%s", pcString );
        fflush( stdout );
    }
    xTaskResumeScheduler();
    /* Allow any key to stop the application running. A real application that
    actually used the key value should protect access to the keyboard input too. */
    if( kbhit() )
    {
        vTaskEndScheduler();
    }
}
```

## 互斥量 Mutex
互斥量是一种特殊的二值信号量，用于控制在两个或多个任务间访问共享资源。

一个任务想要合法地访问资源，其必须先成功地得到(Take)该资源对应的令牌(成为令牌持有者)。
当令牌持有者完成资源使用，其必须马上归还(Give)令牌。

##### 互斥量与二值信号量的小区别
- 用于互斥的信号量必须归还。
- 用于同步的信号量通常是完成同步之后便丢弃，不再归还。

#### 创建互斥量

```
xSemaphoreHandle xSemaphoreCreateMutex( void );
//返回非 NULL 值表示互斥量创建成功。返回值应当保存起来作为该互斥量的句柄.
```

#### 获取和归还互斥量
除了中断函数，所有的信号量都可以通过xSemaphoreTake和xSemaphoreGive函数来获取和归还/给与。（这在前面的信号量中有说明）

#### 互斥量使用示例

```
static void prvNewPrintString( const portCHAR *pcString )
{
    /* 互斥量在调度器启动之前就已创建，所以在此任务运行时信号量就已经存在了。
    试图获得互斥量。如果互斥量无效，则将阻塞，进入无超时等待。 xSemaphoreTake()只可能在成功获得互
    斥量后返回，所以无需检测返回值。如果指定了等待超时时间，则代码必须检测到xSemaphoreTake()返回
    pdTRUE后，才能访问共享资源(此处是指标准输出)。 */
    xSemaphoreTake( xMutex, portMAX_DELAY );
    {
        /* 程序执行到这里表示已经成功持有互斥量。现在可以自由访问标准输出，因为任意时刻只会有一个任
        务能持有互斥量。 */
        printf( "%s", pcString );
        fflush( stdout );
        /* 互斥量必须归还! */
    }
    xSemaphoreGive( xMutex );
    /* Allow any key to stop the application running. A real application that
    actually used the key value should protect access to the keyboard too. A
    real application is very unlikely to have more than one task processing
    key presses though! */
    if( kbhit() )
    {
        vTaskEndScheduler();
    }
}

static void prvPrintTask( void *pvParameters )
{
    char *pcStringToPrint;
    /* Two instances of this task are created so the string the task will send
    to prvNewPrintString() is passed into the task using the task parameter.
    Cast this to the required type. */
    pcStringToPrint = ( char * ) pvParameters;
    for( ;; )
    {
        /* Print out the string using the newly defined function. */
        prvNewPrintString( pcStringToPrint );
        /* 等待一个伪随机时间。注意函数rand()不要求可重入，因为在本例中rand()的返回值并不重要。但
        在安全性要求更高的应用程序中，需要用一个可重入版本的rand()函数 – 或是在临界区中调用rand()
        函数。 */
        vTaskDelay( ( rand() & 0x1FF ) );
    }
}

int main( void )
{
    /* 信号量使用前必须先创建。本例创建了一个互斥量类型的信号量。 */
    xMutex = xSemaphoreCreateMutex();
    /* 本例中的任务会使用一个随机延迟时间，这里给随机数发生器生成种子。 */
    srand( 567 );
    /* Check the semaphore was created successfully before creating the tasks. */
    if( xMutex != NULL )
    {
        /* Create two instances of the tasks that write to stdout. The string
        they write is passed in as the task parameter. The tasks are created
        at different priorities so some pre-emption will occur. */
        xTaskCreate( prvPrintTask, "Print1", 1000,
        "Task 1 ******************************************\r\n", 1, NULL );
        xTaskCreate( prvPrintTask, "Print2", 1000,
        "Task 2 ------------------------------------------\r\n", 2, NULL );
        /* Start the scheduler so the created tasks start executing. */
        vTaskStartScheduler();
    }
    /* 如果一切正常， main()函数不会执行到这里，因为调度器已经开始运行任务。但如果程序运行到了这里，
    很可能是由于系统内存不足而无法创建空闲任务。第五章会提供更多关于内存管理的信息 */
    for( ;; );
}
```

### 互斥量的缺陷
避免优先级反转和死锁的最好方法就是在设计阶段就考虑到这种潜在风险，
这样设计出来的系统就不应该会出现死锁的情况。
#### 1.”优先级反转”

高优先级的任务 2 竟然必须等待低优先级的任务 1 放弃对互斥量的持有权。
高优先级任务被低优先级任务阻塞推迟的行为被称为”优先级反转”。

如果把这种行为再进一步放大，当高优先级任务正等待信号量的时候，一个
介于两个任务优先之间的中等优先级任务开始执行——这就会导致一个高优先级任务
在等待一个低优先级任务，而低优先级任务却无法执行！


##### 一般解决办法：优先级继承
优先级继承暂时地将互斥量持有者的优先级提升至所有等待此互斥量的任务所具
有的最高优先级。互斥量持有者在归还互斥量时，优先级会自动设置为其原
来的优先级。

这种实现假定一个任务在任意时刻只会持有一个互斥量。

#### 2. 死锁
当两个任务都在等待被对方持有的资源时，两个任务都无法再继续执行，这种情况就被称为死锁。

任务 A 在等待一个被任务 B 持有的互斥量，而任务 B 也
在等待一个被任务 A 持有的互斥量。死锁于是发生，因为两个任务都不可能再执行下
去了。


## 守护任务   
> 互斥量缺陷的解决办法

守护任务是对某个资源具有唯一所有权的任务。只有守护任务才可以直接访问其守
护的资源——其它任务要访问该资源只能间接地通过守护任务提供的服务。


#### 采用守护任务的示例
采用了一个守护任务来管理对标准输出的访问。当一个任务想要往终端写信息的时候，其不能直接调用打印函数，而是将消息发送到守护任务。

守护任务使用了一个 FreeRTOS 队列来对终端实现串行化访问。该任务内部实现
不必考虑互斥，因为它是唯一能够直接访问终端的任务。

守护任务大部份时间都在阻塞态等待队列中有信息到来。当一个信息到达时，守护
任务仅仅简单地将收到的信息写到标准输出上，然后又返回阻塞态，继续等待下一条信
息地到来。

```
//守护任务
static void prvStdioGatekeeperTask( void *pvParameters )
{
    char *pcMessageToPrint;
    /* 这是唯一允许直接访问终端输出的任务。任何其它任务想要输出字符串，都不能直接访问终端，而是将要
    输出的字符串发送到此任务。并且因为只有本任务才可以访问标准输出，所以本任务在实现上不需要考虑互斥
    和串行化等问题。 */
    for( ;; )
    {
        /* 等待信息到达。指定了一个无限长阻塞超时时间，所以不需要检查返回值 – 此函数只会在成功收到
        消息时才会返回。 */
        xQueueReceive( xPrintQueue, &pcMessageToPrint, portMAX_DELAY );
        /* 输出收到的字符串。 */
        printf( "%s", pcMessageToPrint );
        fflush( stdout );
        /* Now simply go back to wait for the next message. */
    }
}

static void prvPrintTask( void *pvParameters )
{
    int iIndexToString;
    /* Two instances of this task are created. The task parameter is used to pass
    an index into an array of strings into the task. Cast this to the required type. */
    iIndexToString = ( int ) pvParameters;
    for( ;; )
    {
        /* 打印输出字符串，不能直接输出，通过队列将字符串指针发送到守护任务。队列在调度器启动之前就
        创建了，所以任务执行时队列就已经存在了。并有指定超时等待时间，因为队列空间总是有效。 */
        xQueueSendToBack( xPrintQueue, &( pcStringsToPrint[ iIndexToString ] ), 0 );
        /* 等待一个伪随机时间。注意函数rand()不要求可重入，因为在本例中rand()的返回值并不重要。但
        在安全性要求更高的应用程序中，需要用一个可重入版本的rand()函数 – 或是在临界区中调用rand()
        函数。 */
        vTaskDelay( ( rand() & 0x1FF ) );
    }
}

//心跳钩子函数
//需要设置 FreeRTOSConfig.h 中的常量 configUSE_TICK_HOOK 为 1。和使用函数原型 void vApplicationTickHook( void );
void vApplicationTickHook( void )
{
    static int iCount = 0;
    portBASE_TYPE xHigherPriorityTaskWoken = pdFALSE;
    /* Print out a message every 200 ticks. The message is not written out
    directly, but sent to the gatekeeper task. */
    iCount++;
    if( iCount >= 200 )
    {
        /* In this case the last parameter (xHigherPriorityTaskWoken) is not
        actually used but must still be supplied. */
        xQueueSendToFrontFromISR( xPrintQueue,&( pcStringsToPrint[ 2 ] ),&xHigherPriorityTaskWoken );
        /* Reset the count ready to print out the string again in 200 ticks
        time. */
        iCount = 0;
    }
}

/* 定义任务和中断将会通过守护任务输出的字符串。 */
static char *pcStringsToPrint[] =
{
"Task 1 ****************************************************\r\n",
"Task 2 ----------------------------------------------------\r\n",
"Message printed from the tick hook interrupt ##############\r\n"
};
/*-----------------------------------------------------------*/
/* 声明xQueueHandle变量。这个变量将会用于打印任务和中断往守护任务发送消息。 */
xQueueHandle xPrintQueue;
/*-----------------------------------------------------------*/
int main( void )
{
    /* 创建队列，深度为5，数据单元类型为字符指针。 */
    xPrintQueue = xQueueCreate( 5, sizeof( char * ) );
    /* 为伪随机数发生器产生种子。 */
    srand( 567 );
    /* Check the queue was created successfully. */
    if( xPrintQueue != NULL )
    {
        /* 创建任务的两个实例，用于向守护任务发送信息。任务入口参数传入需要输出的字符串索引号。这两
        个任务具有不同的优先级，所以高优先级任务有时会抢占低优先级任务。 */
        xTaskCreate( prvPrintTask, "Print1", 1000, ( void * ) 0, 1, NULL );
        xTaskCreate( prvPrintTask, "Print2", 1000, ( void * ) 1, 2, NULL );
        /* 创建守护任务。这是唯一一个允许直接访问标准输出的任务。 */
        xTaskCreate( prvStdioGatekeeperTask, "Gatekeeper", 1000, NULL, 0, NULL );  //优先级很低
        /* Start the scheduler so the created tasks start executing. */
        vTaskStartScheduler();
    }
    /* 如果一切正常， main()函数不会执行到这里，因为调度器已经开始运行任务。但如果程序运行到了这里，
    很可能是由于系统内存不足而无法创建空闲任务。第五章会提供更多关于内存管理的信息 */
    for( ;; );
}
```

守护任务的优先级低于打印任务——所以发送到守护任务的消息会一直保持在队
列中，直到两个打印任务都进入阻塞态。在一些情况下，需要给守护任务赋予一个较高
的优先级，消息就可以得到更快的处理——但这样做会由于守护任务的开销使得低优先
级任务被推迟，直到守护任务完成对受其保护的资源的访问。

#### 问题1：
> 资源会被抢占，那任务在对队列读写时，会不会被中断和其他任务打断？

答案：不会，因为队列的特性是读写时会阻塞。

- 由于队列可以被多个任务写入，所以对单个队列而言，也可能有多个任务处于阻塞
状态以等待队列空间有效。这种情况下，一旦队列空间有效，只会有一个任务会被解除
阻塞，这个任务就是所有等待任务中优先级最高的任务。
- 由于队列可以被多个任务读取，所以对单个队列而言，也可能有多个任务处于阻塞
状态以等待队列数据有效。这种情况下，一旦队列数据有效，只会有一个任务会被解除
阻塞，这个任务就是所有等待任务中优先级最高的任务。

（任务可以设置读写队列阻塞超时时间）

#### 问题2：
> 心跳钩子函数在读写队列时阻塞，不会不导致心跳中断程序阻塞？

答案：会有可能， 如果设置了阻塞超时时间，在时间范围内就会阻塞。如果没有设置，当读空队列和写满队列的时候，会xQueueSendToFrontFromISR和xQueueReceiveToFrontFromISR函数直接返回。



---

# 第五章 内存管理

#### 内核使用标准的 malloc()与 free()库函数进行动态内存分配的缺点
1. 这两个函数在小型嵌入式系统中可能不可用。
2. 这两个函数的具体实现可能会相对较大，会占用较多宝贵的代码空间。
3. 这两个函数通常不具备线程安全特性。
4. 这两个函数具有不确定性。每次调用时的时间开销都可能不同。
5. 这两个函数会产生内存碎片。
6. 这两个函数会使得链接器配置得复杂。

FreeRTOS 将内存分配作为可移植层面(相对于基本的内核代码部分而言)。
这使得不同的应用程序可以提供适合自身的具体实现。

FreeRTOS 自带有三种 pvPortMalloc()与 vPortFree()实现范例，
这三个范例对应三个源文件： heap_1.c， heap_2.c， heap_3.c——这三个文件都
放在目录 FreeRTOS\Source\Portable\MemMang 中。

在小型嵌入式系统中，通常是在启动调度器之前创建任务，队列和信号量。这种情
况表明，动态分配内存只会出现在应用程序真正开始执行实时功能之前，而且内存一旦
分配就不会再释放。这就意味着选择内存分配方案时不必考虑一些复杂的因素，比如确
定性与内存碎片等，而只需要从性能上考虑，比如代码大小和简易性。

## 本章目的
- FreeRTOS 在什么时候分配内存。
- FreeRTOS 提供的三种内存分配方案范例。

## 内存分配方案范例

#### Heap_1.c
Heap_1.c 实现了一个非常基本的 pvPortMalloc()版本，而且没有实现 vPortFree()。
如果应用程序不需要删除任务，队列或者信号量，则具有使用 heap_1 的潜质。 Heap_1
总是具有确定性。

##### 分配方案：
将 FreeRTOS 的内存堆空间看作一个简单的数组。当调用
pvPortMalloc()时，则将数组又简单地细分为更小的内存块。

数组的总大小(字节为单位)在 FreeRTOSConfig.h 中由 configTOTAL_HEAP_SIZE
定义。

这种方式定义一个巨型数组会让整个应用程序看起来耗费了许多内存。

#### Heap_2.c
Heap_2.c 也是使用了一个由 configTOTAL_HEAP_SIZE 定义大小的简单数组。不同于 heap_1 的是， heap_2 采用了一个最佳匹配算法来分配内存，并且支持内存释放。

最佳匹配算法保证 pvPortMalloc()会使用最接近请求大小的空闲内存块。

Heap_2.c 虽然不具备确定性，但是比大多数标准库实现的 malloc()与 free()更有效率，
不过静态数组任然会让整个应用程序耗费了许多内存。

#### Heap_3.c
Heap_3.c 简单地调用了标准库函数 malloc()和 free()，但是通过暂时挂起调度器使
得函数调用备线程安全特性。

```
void *pvPortMalloc( size_t xWantedSize )
{
    void *pvReturn;
    vTaskSuspendAll();
    {
        pvReturn = malloc( xWantedSize );
    }
    xTaskResumeAll();
    return pvReturn;
}

void vPortFree( void *pv )
{
    if( pv != NULL )
    {
        vTaskSuspendAll();
        {
            free( pv );
        }
        xTaskResumeAll();
    }
}
```


---

# 错误排查


## printf-stdarg.c
当调用标准 C 库函数时，栈空间使用量可能会急剧上升，特别是 IO 与字符串处理
函数，比如 sprintf()。在 FreeRTOS 下载包中有一个名为 printf-stdarg.c 的文件。这个
文件实现了一个栈效率优化版的小型 sprintf()，可以用来代替标准 C 库函数版本。

## 栈溢出
FreeRTOS 提供了多种特性来辅助跟踪调试栈相关的问题。

#### 任务的运行历史栈侦测
查询指定任务的运行历史中， 其栈空间还差多少就要溢出。

```
unsigned portBASE_TYPE uxTaskGetStackHighWaterMark( xTaskHandle xTask );  //被查询任务的句柄
//返回从任务启动执行开始的运行历史中，栈空间具有的最小剩余量。
//这个值越是接近 0，则这个任务就越是离栈溢出不远了。
```

#### 运行时栈侦测
FreeRTOS 包含两种运行时栈侦测机制，由 FreeRTOSConfig.h 中的配置常量
==configCHECK_FOR_STACK_OVERFLOW== 进行控制。这两种方式都会增加上下切换开销。

栈溢出钩子函数(或称回调函数)由内核在侦测到栈溢出时调用。要使用栈溢出钩子
函数，需要进行以下配置：

- FreeRTOSConfig.h 中把 configCHECK_FOR_STACK_OVERFLOW 设为 1 或 2。
- 提供钩子函数的具体实现，采用程序清单所示的函数名和函数原型。
    ```
    void vApplicationStackOverflowHook( xTaskHandle *pxTask, signed portCHAR *pcTaskName );
    ```
    
- 方法 1

当 configCHECK_FOR_STACK_OVERFLOW 设置为 1 时选用方法 1。

内核会在任务上下文保存后检查栈指针是否还指向有效栈空间。一旦检测到栈指
针的指向已经超出任务栈的有效范围，栈溢出钩子函数就会被调用。

方法 1 具有较快的执行速度，但栈溢出有可能发生在两次上下文保存之间，这种情
况不会被侦测到。

- 方法 2

将 configCHECK_FOR_STACK_OVERFLOW 设为 2 就可以选用方法 2。

当创建任务时，任务栈空间中就预置了一个标记。方法 2 会检查任务栈的最后 20
个字节，查看预置在这里的标记数据是否被覆盖。如果最后 20 个字节的标记数据与预
设值不同，则栈溢出钩子函数就会被调用。

方法 2 没有方法 1 的执行速度快，但测试仅仅 20 个字节相对来说也是很快的。
这种方法应该可以侦测到任何时候发生的栈溢出。


## 其他错误

#### 在中断中调用一个 API 函数，导致应用程序崩溃。
除了具有后缀为”FromISR”函数名的 API 函数，千万不要在中断服务例程中调用其
它 API 函数。

#### 时候应用程序会在中断服务例程中崩溃。
检查中断是否导致了栈溢出。

检查在中断服务例程中使用的语法，宏和调用约定是否符合
Demo 程序的文档描述。

如果应用程序工作在 Cotex M3 上，需要确定给中断指派优先级时，使用低优先级
号数值表示逻辑上的高优先级中断。

#### 在调度器挂起时调用 API 函数，导致应用程序崩溃
调用 vTaskSuspendAll()使得调度器挂起，而唤醒调度器调用 xTaskResumeAll()。
千万不要在调度器挂起时调用其它 API 函数。









