---
title: Qt 信号默认支持的类型
date: 2020-06-16 13:34:08
categories: "Qt"
tags:
	- Qt
---
有时候不知道这个类型的信号支持不支持，需要翻翻文档，隔得太久了，翻了很久才翻到，已经好几次这样了，还是记录下来好点。省得找来找去。
<!-- more -->

## QMetaType类
&emsp;&emsp;元对象类型类

## Detailed Description
&emsp;&emsp;The QMetaType class manages named types in the meta-object system.
The class is used as a helper to marshall types in QVariant and in queued signals and slots connections. It associates a type name to a type so that it can be created and destructed dynamically at run-time. Declare new types with Q_DECLARE_METATYPE() to make them available to QVariant and other template-based functions. Call qRegisterMetaType() to make types available to non-template based functions, such as the queued signal and slot connections.
Any class or struct that has a public default constructor, a public copy constructor, and a public destructor can be registered.
The following code allocates and destructs an instance of MyClass:

  int id = QMetaType::type("MyClass");
  if (id != QMetaType::UnknownType) {
      void *myClassPtr = QMetaType::create(id);
      ...
      QMetaType::destroy(id, myClassPtr);
      myClassPtr = 0;
  }

&emsp;&emsp;If we want the stream operators operator<<() and operator>>() to work on QVariant objects that store custom types, the custom type must provide operator<<() and operator>>() operators.
See also Q_DECLARE_METATYPE(), QVariant::setValue(), QVariant::value(), and QVariant::fromValue().

## Member Type Documentation
&emsp;&emsp;enum QMetaType::Type
These are the built-in types supported by QMetaType:

| Constant                        | Description                       |
| --------------------------------| ----------------------------------|
| QMetaType::Void                 | void                              |              
| QMetaType::Bool                 | bool                              |
| QMetaType::Int                  | int                               |
| QMetaType::UInt                 | unsigned int                      |
| QMetaType::Double               | double                            |
| QMetaType::QChar                | Qchar                             |
| QMetaType::QString              | QString                           |
| QMetaType::QByteArray           | QByteArray                        |
| QMetaType::Nullptr              | std::nullptr_t                    |
| QMetaType::VoidStar             | void *                            |
| QMetaType::Long                 | long                              |
| QMetaType::LongLong             | long long int                     |
| QMetaType::Short                | short                             |
| QMetaType::Char                 | char                              |
| QMetaType::ULong                | usigned long int                  |
| QMetaType::ULongLong            | usinged long long int             |
| QMetaType::UShort               | usigned short                     |
| QMetaType::SChar                | signed char                       |
| QMetaType::UChar                | usigned char                      |
| QMetaType::Float                | float                             |
| QMetaType::QObjectStar          | QObject *                         |
| QMetaType::QVariant             | QVariant                          |
| QMetaType::QCursor              | QCurrsor                          |
| QMetaType::QDate                | QDate                             |
| QMetaType::QSize                | QSize                             |
| QMetaType::QTime                | QTime                             |
| QMetaType::QVariantList         | QVariantList                      |
| QMetaType::QPolygon             | QPolygon                          |
| QMetaType::QPolygonF            | QPloygonF                         |
| QMetaType::QColor               | QColor                            |
| QMetaType::QSizeF               | QSizeF                            |
| QMetaType::QRectF               | QRectF                            |
| QMetaType::QLine                | QLine                             |
| QMetaType::QTextLength          | QTextLength                       |
| QMetaType::QStringList          | QStringList                       |
| QMetaType::QVariantMap          | QVariantMap                       |
| QMetaType::QVariantHash         | QVariantHash                      |
| QMetaType::QIcon                | QIcon                             |
| QMetaType::QPen                 | QPen                              |
| QMetaType::QLineF               | QLineF                            |
| QMetaType::QTextFormat          | QTextFormat                       |
| QMetaType::QRect                | QRect                             |
| QMetaType::QPoint               | QPoint                            |
| QMetaType::QUrl                 | QUrl                              |
| QMetaType::QRegExp              | QRegExp                           |
| QMetaType::QRegularExpression   | QRegularExpression                |
| QMetaType::QDateTime            | QDateTime                         |
| QMetaType::QPointF              | QPointF                           |
| QMetaType::QPalette             | QPalette                          |
| QMetaType::QFont                | QFont                             |
| QMetaType::QBrush               | QBursh                            |
| QMetaType::QRegion              | QRegion                           |                            
| QMetaType::QBitArray            | QBitArray                         |
| QMetaType::QImage               | QImage                            |
| QMetaType::QKeySequence         | QKeySequence                      |
| QMetaType::QSizePolicy          | QSizePolicy                       |
| QMetaType::QPixmap              | QPixmap                           |
| QMetaType::QLocale              | QLocale                           |
| QMetaType::QBitmap              | QBitmap                           |
| QMetaType::QMatrix              | QMatrix                           |
| QMetaType::QTransform           | QTransform                        |
| QMetaType::QMatrix4x4           | QMatrix4x4                        |
| QMetaType::QVector2D            | QVector2D                         |
| QMetaType::QVector3D            | QVector3D                         |
| QMetaType::QVector4D            | QVector4D                         |
| QMetaType::QQuaternion          | QQuaternion                       |
| QMetaType::QEasingCurve         | QEasingCurve                      |
| QMetaType::QJsonValue           | QJsonValue                        |
| QMetaType::QJsonObject          | QJsonObject                       |
| QMetaType::QJsonArray           | QJsonArray                        |
| QMetaType::QJsonDocument        | QJsonDocument                     |
| QMetaType::QModelIndex          | QModelIndex                       |
| QMetaType::QPersistentModelIndex| QPersistentModelIndex             |
| QMetaType::QUuid                | QUuid                             |
| QMetaType::QByteArrayList       | QByteArrayList                    |
| QMetaType::User                 | Base value for user types         |
| QMetaType::UnknownType          | 太长放下面解释                     |
QMetaType::UnknownType： This is an invalid type id. It is returned from QMetaType for types that are not registered
Additional types can be registered using Q_DECLARE_METATYPE().
See also type() and typeName().
