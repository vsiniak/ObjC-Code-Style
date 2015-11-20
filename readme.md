# Objective-C code style

## Введение

### Цели

Цель этого документа -- описать единый code style для разработчиков компании.

Цель единого code style -- увеличить степень комфорта и продуктивности совместной работы команд разработчиков.

Цель этого code style -- улучшить понятность и читаемость кода, а также его гибкость (т.е. простоту изменения) и стабильность.

### Примечания

Правила этого code style можно нарушить на тех участках кода, где это улучшит читаемость.

Не забудьте про возможности автоформатирования кода в вашей среде разработки.

## Источники

* iOS SDK Code style
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Code Complete: A Practical Handbook of Software Construction, Second Edition](http://cc2e.com/) (Steve McConnell)
* Личный опыт 

## Оглавление

* [Имена](#Имена)
* [Пробелы и отступы](#Пробелы-и-отступы)
* [Пустые строки](#Пустые-строки)
* [Выравнивание](#Выравнивание)
* [Операторные скобки](#Операторные-скобки)
* [Константы](#Константы)
* [Методы](#Методы)
* [Свойства](#Свойства)
* [Классы и протоколы](#Классы-и-протоколы)
* [Точки возврата из функции](#Точки-возврата-из-функции)
* [Конструкторы](#Конструкторы)
* [Булевы переменные](#Булевы-переменные)
* [Литералы](#Литералы)
* [Перечисления](#Перечисления)
* [`switch`](#switch)
* [Тернарный оператор](#Тернарный-оператор)
* [`.`-нотация](#-нотация)
* [Приватные свойства](#Приватные-свойства)
* [Структура классов](#Структура-классов)
* [Комментарии](#Комментарии)

## Имена

* Используйте camel-case.

**Хорошо**

```objc
extern NSString *const OSItemNotificationKey;
NSUInteger itemsCount;
NSTimeInterval animationDuration;
```

**Плохо**

```objc
extern NSString *const OS_ITEM_NOTIFICATION_KEY;
NSUInteger items_count;
NSTimeInterval animation_duration;
```

* Названия классов, протоколов, перечислений (`enum`) и констант начинаются с заглавной буквы.
* Названия классов начинаются с 2-3-символьного префикса, соответствующего проекту. (Так просто искать классы и файлы проекта.)
* Названия констант имеют префикс в виде названия класса, к которому константы относятся.

**Хорошо**

```objc
static NSString *const OSItemDidUpdateNotification = @"OSItemDidUpdateNotification";

@implementation OSItem

#pragma mark - Private methods

- (void)update
{
    [self.notificationCenter postNotificationName:OSItemDidUpdateNotification object:self];
}

@end
```

**Плохо**

```objc
static NSString *const notification = @"notification";

@implementation OSItem

#pragma mark - Private methods

- (void)update
{
    [self.notificationCenter postNotificationName:notification object:self];
}

@end
```

* Названия полей класса начинаются с префикса `_`.

**Хорошо**

```objc
@interface OSItem()

@property(nonatomic, strong) NSString *title;

@end

@implementation OSItem
{
    NSString *_uppercaseTitle;
}

...

@end
```

**Плохо**

```objc
@interface OSItem()

@property(nonatomic, strong) NSString *m_title;

@end

@implementation OSItem
{
    NSString *m_uppercaseTitle_;
}

...

@end
```

* Символ `*` для обозначения указателя ставится ближе к переменной.

**Хорошо**

```objc
@property(nonatomic, strong) UIView *nameView;
```

**Плохо**

```objc
@property(nonatomic, strong) UIView* nameView;
@property(nonatomic, strong) UIView * titleView;
```

* Используйте максимально понятные названия без общепринятых сокращений. Добавляйте суффикс для объяснения типа объекта, где нужно.
* Имена переменных типа `BOOL` начинайте с `is`/`has` и т.п.

**Хорошо**

```objc
@property(nonatomic, strong) UIView *nameView;
@property(nonatomic, assign) NSInteger itemsCount;
@property(nonatomic, assign) BOOL isLoggedIn;

- (void)addItem:(OSItem *)item;
```
**Плохо**

```objc
@property(nonatomic, strong) UIView *name;
@property(nonatomic, assign) NSInteger count;
@property(nonatomic, assign) BOOL login;

- (void)add:(OSItem *)i;
```

## Пробелы и отступы

* Делайте отступ, состоящий из 4 пробелов. Не используйте символ табуляции для отступов.
* Пробел ставится:
    * слева и справа от бинарных операторов (`+`/`-`/`*`/`/`/`&`/`&&` и т.д.)
* Пробел не ставится:
    * между унарным оператором (`++`/`--`) и операндом
    * после управляющих операторов (`if`/`else`/`switch`/`while`)
    * после ключевого слова `@property`

## Пустые строки

Одной пустой строкой выделяются:

* Объявления и реализации классов
* Группы объявлений методов и свойств
* Реализации методов
* Заголовки секций файла (`pragma mark`)

```objc
//OSItemView.h

@interface OSItemView: UIView

@property(nonatomic, strong) NSString *title;
@property(nonatomic, strong) NSString *subtitle;
@property(nonatomic, strong) NSString *name;

@property(nonatomic, assign) BOOL isAnimating;

- (NSString *)capitalizedTitle;

- (void)startAnimation;
- (void)endAnimation;

@end
```

```objc
// OSItemView.m

@implementation OSItemView

#pragma mark - Interface methods
#pragma mark * common

- (NSString *)capitalizedTitle
{
    ...
}

#pragma mark * animation

- (void)startAnimation
{
    ...
}

- (void)endAnimation
{
    ...
}

@end
```

## Выравнивание

* Объявления переменных и свойств не выравниваются по колонке. Это усложняет поддержку кода.

**Хорошо**

```objc
@property(nonatomic, strong) NSString *title;
@property(nonatomic, assign) NSUInteger *count;
@property(nonatomic, copy) NSArray *items;
```

**Плохо**

```objc
@property(nonatomic, strong) NSString   *title;
@property(nonatomic, assign) NSUInteger *count;
@property(nonatomic, copy)   NSArray    *items;
```

* Сигнатуры методов не выравниваются в колонку по `:`. Это усложняет поддержку кода и не стимулирует писать лаконичные сигнатуры методов, при этом едва ли улучшая читаемость.

**Хорошо**

```objc
[UIView animateWithDuration:1.0 animations:^
{
    self.frame = CGRectZero;
}
completion:^(BOOL finished)
{
    self.isAnimating = NO;
}];
```

**Плохо**

```objc
[UIView animateWithDuration:1.0
                 animations:^
                 {
                     self.frame = CGRectZero;
                 }
                 completion:^(BOOL finished)
                 {
                     self.isAnimating = NO;
                 }];
```

## Операторные скобки

* Операторные скобки ставятся всегда, даже когда блок состоит из одной инструкции.

**Хорошо**

```objc
if(isAnimating)
{
    self.title = @"Loading...";
    self.subtitle = @"Please wait...";
}
```

**Плохо**

```objc
if(isAnimating)
    self.title = @"Loading...";
    self.subtitle = @"Please wait..."; // Ошибка
```

* Операторные скобки всегда начинаются с новой строки (в т.ч. когда используются для объявления классов). Это делает код более читабельным.

**Хорошо**

```objc
- (BOOL)canEnableNotifications
{
    if([sharedApplication respondsToSelector:@selector(currentUserNotificationSettings)])
    {
        if((currentUserNotificationTypes & UIUserNotificationTypeAlert) != UIUserNotificationTypeAlert)
        {
            return NO;
        }
    }
    return YES;
}
```

**Плохо**

```objc
- (BOOL)canEnableNotifications {
    if([sharedApplication respondsToSelector:@selector(currentUserNotificationSettings)]) {
        if((currentUserNotificationTypes & UIUserNotificationTypeAlert) != UIUserNotificationTypeAlert) {
            return NO;
        }
    }
    return YES;
}
```

## Константы

* Используйте константы (`constant`) вместо директив (`define`).
* Помечайте константы ключевым словом `static`.

**Хорошо**

```objc
static NSString *const OSItemNameKey = @"OSItemNameKey";
static NSTimeInterval const OSItemAnimationDuration = 2.0f;
```

**Плохо**

```objc
#define NAME_KEY @"name"
#define AnimationDuration 2
```

* Используйте ключевое слово `extern` для констант, объявленных в файле заголовка.

```objc
// OSItem.h

extern NSString *const OSItemNameKey;
extern NSTimeInterval const OSItemAnimationDuration;
```

```objc
// OSItem.m

NSString *const OSItemNameKey = @"OSItemNameKey";
NSTimeInterval const OSItemAnimationDuration = 2.0f;
```

## Методы

* В сигнатурах метода после символов `-`/`+` ставится пробел; после закрывающей скобки пробел не ставится.
* При передаче нескольких параметров не нужно объединять их, используя слова `and`/`with` и т.д. Но используйте эти слова, если они описывают метод.

**Хорошо**

```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
- (void)animateWithCompletionBlock:(OSBlock)completionBlock;
- (UIView *)viewWithTag:(NSInteger)tag;
```

**Плохо**

```objc
-(void) setExampleText:(NSString *)text andImage:(UIImage *)image;
-( instancetype )initWithWidth:(CGFloat)width withHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;
-(void)setT:(NSString *)text i:(UIImage *)image;
- (UIView *) view:(NSInteger)tag;
```

## Свойства

* Между ключевым словом `@property` и скобкой не ставится пробел. Модификаторы свойства разделяются запятой и пробелом.
* Модификаторы свойства идут в следующем порядке (ненужные модификаторы опускаются):

    1. `nonatomic`
    2. `assign`/`weak`/`strong` etc.
    3. `nullable`/`nonnull` etc.
    4. `getter=<getter>`
    5. `setter=<setter>`

**Хорошо**

```objc
@property(nonatomic, assign, setter=animated) BOOL isAnimated;
```

**Плохо**
   
```objc
@property (strong,nonnull, nonatomic) NSString *name;
```
    
* Приватные свойства должны быть объявлены в анонимной (без имени) категории класса в файле реализации.
* Свойства, публично доступные только для чтения, приватно доступные для редактирования, должны быть объявленными только для чтения в файле заголовка и переопределены в файле реализации.

```objc
// OSItem.h

@interface OSItem: NSObject

@property(nonatomic, strong, readonly) NSNumber *itemId;
@property(nonatomic, strong) NSString *title;

@end
```

```objc
// OSItem.m

@interface OSItem()

@property(nonatomic, strong, readwrite) NSNumber *itemId;
@property(nonatomic, assign) BOOL isLoading;

@end
```

## Классы и протоколы

* Символ наследования `:` выделяется пробелами с двух сторон.
* На скобки протоколов `<``>` распространяются те же правила, что и на операторные скобки. Так нагляднее видны реализуемые протоколы, а также удобнее их редактировать.

```objc
@interface OSItem : NSObject
<
    NSCopying,
    NSMutableCopying
>

@end
```

* При описании протокола ключевые слова `@optional` и `@required` выделяются пустыми строками.

```objc
@protocol OSItem

@required

- (NSString *)itemName;
- (NSString *)itemCategory;

@end
```

* При объявлении переменной, реализующей протокол, между типом и скобкой `<` пробел не ставится. Внутри скобок `<``>` протоколы разделяются запятой и пробелом.

**Хорошо**

```objc
@property(nonatomic, weak) id<OSItemDelegate, NSCopying> delegate;
```

**Плохо**

```objc
@property(nonatomic, weak) id < OSItemDelegate,NSCopying > delegate;
```

## Точки возврата из функции

* Правило золотого пути: отступ внутри функции должен быть минимальным, поэтому нужно избегать оборота кода в несколько `if`: вместо этого можно проверить предусловия и завершить метод, если нужно.

**Хорошо**

```objc
- (void)exampleMethod
{
    if(isProcessed)
    {
        return;
    }
    
    if(hasError)
    {
        return;
    }
    
    ...
}
```

**Плохо**

```objc
- (void)exampleMethod
{
    if(!isProcessed)
    {
        if(!hasError)
        {
            ...
        }
    }
}
```

* Если возвращаемое значение зависит от каких-либо условий, при этом каждое условие возвращает соответствующее значение и обрабатывается в `if`-`else` или `switch`, то можно возвращать значение внутри этих операторов.

```objc
- (NSString *)processResultString
{
    if(isProcessed)
    {
        return @"Alread processed";
    }
    else
    {
        return @"Didn't process";
    }
}

- (NSString *)errorDescription
{
    switch(error)
    {
        case OSItemErrorNotFound:
            return @"Not found";
        
        case OSItemErrorServerError:
            return @"Server error";
        
        default:
            return @"Unknown error";
    }
}
```

* Во всех остальных случаях из функции должна быть только одна точка выхода.

**Хорошо**

```objc
- (NSInteger)salaryTax
{
    NSInteger tax;
    
    tax = salary * 0.3f;
    
    if(!isOnVacation)
    {
        tax /= 1.1f;
    }
    
    if(!isRented && tax < OSItemTaxMedian)
    {
        tax += (OSItemTaxMedian - tax) / 2.0f;
    }
    
    if(tax > OSItemTaxMax)
    {
        tax /= 1.1f;
    }
    
    return tax;
}
```

**Плохо**

```objc
- (NSInteger)salaryTax
{
    NSInteger tax;
    
    tax = salary * 0.3f;
    
    if(!isOnVacation)
    {
        tax /= 1.1f;
        
        if(isRented)
        {
            return tax;
        }
    }
    else if(isRented)
    {
        return (OSItemTaxMedian - tax) / 2.0f;
    }
    
    if(!isRented && tax < OSItemTaxMedian)
    {
        tax += (OSItemTaxMedian - tax) / 2.0f;
    }
    else
    {
        return tax;
    }
    
    if(tax > OSItemTaxMax)
    {
        return tax / 1.1f;
    }
    
    return tax;
}
```

## Конструкторы

* Используйте шаблон iOS SDK.
* Используйте тип возвращаемого значения `instancetype`.

```objc
- (instancetype)init
{
    self = [super init];
    if(self)
    {
        [self initItems];
        [self initViews];   
    }
    return self;
}
```

## Булевы переменные

* Используйте ключевые слова `YES` и `NO` при работе с `BOOL`. (При работе с `C`/`C++`-кодом, и соответственно, с `bool`, используйте `true` и `false`.)
* Имя булевой переменной начинайте с `is`/`has`/`was` и т.д.
* В условных операторах не сравнивайте булевые переменные со значениями.

**Хорошо**

```objc
BOOL isAnimating = NO;
BOOL isLoading = YES;

if(isAnimating && !isLoading)
{
    ...
}
```

**Плохо**

```objc
BOOL animated = 0;
BOOL loading = true;

if(animated == YES && loading == NO)
{
    ...
}
```

## Литералы

* Используйте литералы для создания неизменяемых объектов классов `NSString`, `NSDictionary`, `NSArray` и `NSNumber`, где это возможно. При создании литералов `NSArray` и `NSDictionary` помните, что попытка использовать нулевой указатель приведёт к крэшу.
* При создании литералов и объектов `NSArray` и `NSDictionary`, содержащих больше одного-двух объектов, используйте переносы строк.

```objc
NSArray *actions = @[@"Add", @"Delete"];
NSArray *actionsDescriptions = @[
    @"Add item to array",
    @"Remove item from array",
    @"Show help"
];
NSDictionary *devicesCodes = @{
    @"IPH": @"iPhone",
    @"IPD": @"iPad",
    @"IP": @"iPod"
};
NSNumber *isAnimating = @YES;
NSNumber *animationDuration = @(1.1f * 2.0f);
```

* Не используйте литералы для создания изменяемых объектов.

**Хорошо**

```objc
NSMutableArray *sliderValues = [NSMutableArray arrayWithItems:
    @"Off",
    @"Low",
    @"Medium",
    @"High",
    @"Very high",
    nil];
NSMutableDictionary *sliderTitles = [NSMutableDictionary dictionary];
```

**Плохо**

```objc
NSMutableArray *sliderValues = @[@0, @2, @4].mutableCopy;
NSMutableDictionary *sliderTitles = @{}.mutableCopy;
```

## Перечисления

* Используйте перечисления, когда переменная может принимать одно из нескольких значений, имеющих определённый смысл.
* Используйте `NS_ENUM` и `NS_OPTIONS` вместо `enum` -- этот тип лучше поддерживается Xcode (автоподстановка, анализ кода).

**Хорошо**

```objc
typedef NS_ENUM(NSUInteger, OSItemError)
{
    OSItemErrorNotFound = 404,
    OSItemErrorNoError = 200,
    OSItemErrorServerError = 500
};

@implementation OSItem

...

- (void)processError:(OSItemError)error
{
    switch(error)
    {
        case OSItemErrorNotFound:
            ...
            break;
            
        case OSItemErrorNoError:
            ...
            break;
            
        case OSItemErrorServerError:
            ...
            break;
        
        // default-case не нужен, т.к. обработаны все возможные значения переменной типа OSItemError
    }
}

@end
```

**Плохо**

```objc
static NSUInteger const OSItemErrorNotFound = 404;
static NSUInteger const OSItemErrorNoError = 200;
static NSUInteger const OSItemErrorServerError = 500;

@implementation OSItem

...

- (void)processError:(NSUInteger)error
{
    switch(error)
    {
        case OSItemErrorNotFound:
            ...
            break;
            
        case OSItemErrorNoError:
            ...
            break;
            
        case OSItemErrorServerError:
            ...
            break;
            
        default:
            break;
    }
}

@end
```

## `switch`

* Операторные скобки ставятся тогда, когда это необходимо.
* При прохождении через несколько `case`, необходимо отмечать это в комментарии, например, `// fallthrough`.
* При использовании `switch` с переменной типа перечисления, блок `default` не нужен, если перечислены все возможные значения переменной.

```objc
typedef NS_ENUM(NSUInteger, OSItemError)
{
    OSItemErrorNotFound = 404,
    OSItemErrorNoError = 200,
    OSItemErrorServerError = 500
};

@implementation OSItem

...

- (void)processError:(OSItemError)error
{
    BOOL isProcessed = NO;
    NSString *errorDescription = nil;

    switch(error)
    {
        case OSItemErrorNotFound:
            isProcessed = YES;
            errorDescription = @"Not found";
            // fallthrough
            
        case OSItemErrorNoError:
        {
            NSString *name = self.name;
            errorDescription = [name stringByAppendingString:@" error"];
        }
            break;
            
        case OSItemErrorServerError:
            isProcessed = NO;
            break;
    }
}

@end
```

## Тернарный оператор

* Используйте тернарный оператор вместо `if` в простых случаях присваивания или возврата из функции значения.
* Если какая-либо из частей тернарного оператора содержит несколько операндов, часть должна оборачиваться в скобки. И в этом случае нужно задуматься об использовании `if`.

**Хорошо**

```objc
- (NSString *)exampleMethod
{
    NSString *alertTitle;
    NSString *alertDescription;
    
    if(self.hasError)
    {
        alertTitle = @"Error";
        alertDescription = @"Error occured";
    }
    
    if(alertTitle != nil)
    {
        [self showAlert];
    }
    
    NSString *errorDescription = (self.hasError && alertTitle != nil) ? alertTitle : @"No error";
    
    return (errorDescription != nil) ? errorDescription : self.alertTitle;
}
```

**Плохо**

```objc
- (NSString *)exampleMethod
{
    NSString *alertTitle = self.hasError ? @"Error" : nil;
    NSString *alertDescription = self.hasError ? @"Error occured" : nil;
    
    self.alertTitle != nil ? [self showAlert] : NULL;
    
    return self.hasError && alertTitle != nil ? alertDescription != nil ? alertDescription : alertTitle : @"No error";
}
```

## `.`-нотация

* Используйте `.`-нотацию для обращения к свойствам.

**Хорошо**

```objc
@interface OSItem()

@property(nonatomic, strong) NSString *title;

@end

@implementation OSItem()

#pragam mark - Private methods

- (void)doSomethingWithTitle
{
    NSString *oldTitle = self.title;
    self.title = oldTitle.uppercaseString;
}

@end
```

**Плохо**

```objc
@interface OSItem()

@property(nonatomic, strong) NSString *title;

@end

@implementation OSItem()

#pragam mark - Private methods

- (void)doSomethingWithTitle
{
    NSString *oldTitle = [self title];
    [self setTitle:oldTitle.uppercaseString];
}

@end
```

* Так как для свойств, объявленных как `@property`, просто генерируются соответствующие методы, используйте `.`-нотацию также для методов, аналогичных геттерам (методы, не принимающие аргументов и возвращающие значение, по смыслу являющиеся геттерами).

**Хорошо**

```objc
- (NSString *)displayName
{
    return [self.name stringByAppendingString:self.surname];
}

- (void)showDisplayName
{
    [self showAlertWithText:self.displayName];
}
```

**Плохо**

```objc
- (NSString *)createDisplayNameAlert
{
    return [OSAlert alertWithText:self.displayName]; // Тут всё хорошо
}

- (void)showDisplayName
{
    self.createDisplayNameAlert.show; // Тут плохо
}
```

* Вызывайте явно методы во всех остальных случаях.

## Структура классов

Используйте `#pragma mark - <Section title>`, чтобы сгруппировать методы в функциональные блоки. При необходимости разбивайте блоки на подблоки, используя `#pragma mark * <Subsection title>`. (Такой стиль `pragma mark` корректно обрабатывается как Xcode, так и AppCode.)

Функциональные блоки располагайте в следующем порядке:

1. Приватные методы.
2. Переопределённые методы. Методы разных классов располагаются в соответствующих блоках. Порядок блоков совпадает с порядком наследования. Метод относится к блоку того класса, где был объявлен впервые.
3. Методы реализуемых протоколов. Методы разных протоколов располагаются в соответствующих блоках. Порядок блоков совпадает с порядком объявления протоколов.
4. Интерфейсные методы (методы, объявленные в файле заголовка).

```objc
//OSItemView.h

@interface OSItemView: UIView

@property(nonatomic, strong) NSString *title;

- (NSString *)capitalizedTitle;

@end
```

```objc
// OSItemView.m

@interface OSItemView()
<
    NSCopying,
    NSMutableCopying
>

@property(nonatomic, assign) NSUInteger titleLength;

@end

@implementation OSItemView

#pragma mark - Private methods

- (void)countTitleLength
{
    self.titleLength = title.length;
}

#pragma mark - NSObject methods

- (instancetype)init
{
    self = [super init];
    if(self)
    {
        self.titleLength = 0;
    }
    return self;
}

#pragma mark - UIView methods

- (void)sizeToFit
{
    CGRect frame = self.frame;
    frame.size = CGSizeMake(100.0f, 100.0f);
    self.frame = frame;
}

#pragma mark - NSCopying protocol

- (instancetype)copyWithZone:(NSZone *)zone
{
    return nil;
}

#pragma mark - NSMutableCopying protocol

- (instancetype)mutableCopyWithZone:(NSZone *)zone
{
    return nil;
}

#pragma mark - Interface methods
#pragma mark * setters/getters

- (NSString *)title
{
    return (self.title != nil) ? self.title : @"";
}

- (void)setTitle:(NSString *)title
{
    _title = title;
    [self countTitleLength];
}

#pragma mark * methods

- (NSString *)capitalizedTitle
{
    return self.title.uppercaseString;
}

@end
```

## Комментарии

Наличие комментариев усложняет поддержку кода, потому что требуется держать их актуальными. Вместо этого лучше писать самодокументирующийся код:

* Использовать понятные названия методов, переменных и классов.
* Разбивать сложные и большие методы на подметоды.

**Хорошо**

```objc
- (OSItem *)localItemWithId:(NSUInteger)itemId
{
    for(OSItem *item in self.items)
    {
        if(item.id == itemId)
        {
            return item;
        }
    }
    return nil;
}

- (void)mergeLocalItemsWithRemoteItems
{
    for(OSItem *remoteItem in items)
    {
        OSItem *localItem = [self localItemWithId];
        [localItem mergeWithItem:remoteItem];
    }
}

- (void)notifyObservers
{
    for(id<OSObserver> observer in self.observers)
    {
        [objserver dataSourceDidSynchronizeData:self];
    }
}

- (void)synchronizeData
{
    [self.networkConnection getItemsWithCompletionBlock:^(NSArray *items)
    {
        [self mergeLocalItemsWithRemoteItems];
        [self notifyObservers];
    }];
}
```

**Плохо**

```objc
- (void)synchronizeData
{
    [self.networkConnection getItemsWithCompletionBlock:^(NSArray *items)
    {
        for(OSItem *remoteItem in items)
        {
            for(OSItem *localItem in self.items)
            {
                if(remoteItem.id == localItem.id)
                {
                    localItem.name = remoteItem.name;
                    ...
                }
            }
        }
        for(id<OSObserver> observer in self.observers)
        {
            [objserver dataSourceDidSynchronizeData:self];
        }
    }];
}
```

* Использовать средства iOS SDK для документирования сигнатур методов и свойств, где это требуется.

```objc
@property(nonatomic, strong, nonnull) NSMutableArray *items;

- (void)saveWithError:(inout NSError **error);
```

```objc
- (void)addItem:(nonnull OSItem *)item
{
    [self.items addObject:item];
}

- (void)addItemSafely:(nullable OSItem *)item
{
    if(item != nil)
    {
        [self addItem:item];
    }
}
```

* Использовать NSAssert, чтобы описать дополнительные предусловия метода.

```objc
- (NSNumber *)taxForCost:(NSInteger)cost
{
    NSAssert(cost > 0, @"Cost can't be less or equal than zero!");
    return @(sqrt(cost) / 4.0f)
}
```
* Если никакими из предложенных способов нельзя описать код, используйте комментарии.
