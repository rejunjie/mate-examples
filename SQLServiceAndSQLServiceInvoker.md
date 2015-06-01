# Source #

Grab the SWC ( SQLiteExtension_(version-no).swc ) available at ['Downloads'](http://code.google.com/p/mate-examples/downloads/list) or check out the latest source from the [repository](http://code.google.com/p/mate-examples/source/checkout) (folder 'extensions/airsqlite')_

# Usage #

The instructions in the post on [WS-Blog](http://www.websector.de/blog/): [New Mate extensions for using AIR and SQLite: “SQLService + SQLServiceInvoker”](http://www.websector.de/blog/2008/10/04/new-mate-extensions-for-using-air-and-sqlite-sqlservice-sqlserviceinvoker/) demonstrate the original implementation of the SQLite Extention for Mate, and not a lot has changed since. :)


Check out the source of the example called ['airsqlite'](http://code.google.com/p/mate-examples/source/browse/trunk/#trunk/examples/airsqlite)

Key changes into version 0.2 allow for
  * Multiple SQLStatements/Strings to be executed in one transaction
  * Business logic within a manager
  * Prefetch
  * Return of SQLresult, rather than just the data array allowing for the retrieval of lastInsertRowID and wotnot

# Examples #

The key file is still MainEventMap.mxml:

## Executing a String ##
```
	<EventHandlers type="{ UserEvent.DELETE }">
		<air:SQLServiceInvoker
			instance="{ sqlService }"
			sql="{ "delete from users where userId=?" }"
			parameters="{[ event.userId ]}">
			<air:resultHandlers>
				<EventAnnouncer 
						generator="{ UserEvent}" 
				type="{ UserEvent.GET_USERS}" 
					/>
	        </air:resultHandlers>
		</air:SQLServiceInvoker>
	</EventHandlers>
```

Here the sql parameter holds a string that is modified with the elements within the array of parameters

## Executing a Statement ##
```
	<EventHandlers type="{ UserEvent.ADD }">
		<air:SQLServiceInvoker
			instance="{ sqlService }"
			statement="{ sqlManager.addUser }"
			parameters="{[ event.userVO.firstName, event.userVO.lastName ]}">
			<air:resultHandlers>
				<!-- resultObject is an SQLResult. lastInsertRowID can be retrieved -->
				<EventAnnouncer 
					generator="{ UserEvent}" 
					type="{ UserEvent.GET_USERS }" 
					/>
	        </air:resultHandlers>      			
		</air:SQLServiceInvoker>
	</EventHandlers>
```


Here the statement parameter is passed an SQLStatement. It is not strictly necessary to make SQLStatements, and it is only here to demonstrate legacy functionality. Every String that is passed through the sql parameter is converted into an SQLStatement anyway and has the parameters applied to it.


## Executing via Transaction ##
```
	<EventHandlers type="{ UserEvent.INSERT_STATEMENTS }">
		<air:SQLServiceInvoker
			instance="{ sqlService }"
			sqlArray="{ sqlManager.insertStatements }">
			<air:resultHandlers>
				<EventAnnouncer 
					generator="{ UserEvent}" 
					type="{ UserEvent.GET_USERS }" 
					/>
	        </air:resultHandlers>      			
		</air:SQLServiceInvoker>
	</EventHandlers>
```

By putting an array into sqlArray, each of the SQLStatements/Strings within it are run in a batch. Parameters are not applied to the elements of this array and must be applied before the array is passed.


## Implementing Business Logic ##
```
	<EventHandlers type="{ UserEvent.INSERT_STRINGS }">
		<MethodInvoker generator="{ SQLManager }" method="setInsertStrings" arguments="{event.number}" />
		<DataCopier destination="{ sqlManager }" destinationKey="insertStrings" source="{lastReturn}" />
		<air:SQLServiceInvoker
			instance="{ sqlService }"
			sqlArray="{ sqlManager.insertStrings }">
			<air:resultHandlers>
				<EventAnnouncer 
					generator="{ UserEvent}" 
					type="{ UserEvent.GET_USERS }" 
					/>
	        </air:resultHandlers>      			
		</air:SQLServiceInvoker>
	</EventHandlers>
```

If you don't actually know what your SQL statements will be before you call the SQLServiceInvoker and wisely don't want your view to do the work for you, you can use a manager to execute all your business logic before passing it in. This is just one implementation (but it works!)

## Query with Prefetch ##
```

	<EventHandlers type="{ UserEvent.GET_USERS }">
		<air:SQLServiceInvoker
			instance="{ sqlService }"
			itemClass="{ UserVO }"
			sql="{ sqlManager.getAllUsers }"
			prefetch="20">
			<air:resultHandlers>
				<MethodInvoker 
					generator="{ MainModel }"
					method="setUserData" 
					arguments="{ resultObject.data }" 
					/>	
	        </air:resultHandlers>
		</air:SQLServiceInvoker>
	</EventHandlers>

	<EventHandlers type="{ UserEvent.GET_NEXT }">
		<air:SQLServiceInvoker
			instance="{ sqlService }"
			prefetch="20">
			<air:resultHandlers>
				<MethodInvoker 
					generator="{ MainModel }"
					method="addToUserData" 
					arguments="{ resultObject }" 
					/>
	        </air:resultHandlers>
		</air:SQLServiceInvoker>
	</EventHandlers>
```

The first event specifies a prefetch of 20, meaning that only the first 20 records will be returned. The itemClass parameter converts the records returned into that class based on matching names.

The second event, with no sql or statements defined, takes the SQLStatement that still exists in memory and asks for the next 20 records.


## Screen shot ##

[![](http://mate-examples.googlecode.com/svn/trunk/wiki/images/mateAIRSQLiteExample.png)](http://www.websector.de/blog/2008/10/04/new-mate-extensions-for-using-air-and-sqlite-sqlservice-sqlserviceinvoker/)

# To Do #

Functionality enhancements might be...
  * Asynchronous execution (for non-transactions. Transactions are always synchronous)
  * Integration with an Active Record framework
  * Formalised standard for use of Managers


# Author #

Jens Krause: [www.websector.de/blog](http://www.websector.de/blog)

Ben Reynolds: http://likethewolf.net