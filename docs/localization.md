# localization

The `Localization` class is responsible for fetching resources and
formatting translations.

It implements the fallback strategy in case of errors encountered during the
formatting of translations.

In HTML and XUL, l20n.js will create an instance of `Localization` for the
default set of `<link rel="localization">` elements.  You can get
a reference to it via:

    const localization = document.l10n.get('main');

Different names can be specified via the `name` attribute on the `<link>`
elements.  One `document` can have more than one `Localization` instance,
but one `Localization` instance can only be assigned to a single `document`.

# constructor

Create an instance of the `Localization` class.

The instance's configuration is provided by two runtime-dependent
functions passed to the constructor.

The `requestBundles` function takes an array of language codes and returns
a Promise of an array of lazy `ResourceBundle` instances.  The
`Localization` instance will imediately call the `fetch` method of the
first bundle returned by `requestBundles` and may call `fetch` on
subsequent bundles in fallback scenarios.

The array of bundles is the de-facto current fallback chain of languages
and fetch locations.

The `createContext` function takes a language code and returns an instance
of `Intl.MessageContext`.  Since it's also provided to the constructor by
the runtime it may pass runtime-specific `functions` to the
`MessageContext` instances it creates.

**Parameters**

-   `requestBundles` **[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)** 
-   `createContext` **[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)** 

Returns **Localization** 

# requestLanguages

Initiate the change of the currently negotiated languages.

`requestLanguages` takes an array of language codes representing user's
updated language preferences.

**Parameters**

-   `requestedLangs` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** 

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;ResourceBundle>>** 

# formatValues

Retrieve translations corresponding to the passed keys.

A generalized version of `Localization.formatValue`.  Keys can either be
simple string identifiers or `[id, args]` arrays.

    document.l10n.formatValues(
      ['hello', { who: 'Mary' }],
      ['hello', { who: 'John' }],
      'welcome'
    ).then(console.log);

    // ['Hello, Mary!', 'Hello, John!', 'Welcome!']

Returns a Promise resolving to an array of the translation strings.

**Parameters**

-   `keys` **...([Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) \| [string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String))** 

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>>** 

# formatValue

Retrieve the translation corresponding to the `id` identifier.

If passed, `args` is a simple hash object with a list of variables that
will be interpolated in the value of the translation.

    localization.formatValue(
      'hello', { who: 'world' }
    ).then(console.log);

    // 'Hello, world!'

Returns a Promise resolving to the translation string.

Use this sparingly for one-off messages which don't need to be
retranslated when the user changes their language preferences, e.g. in
notifications.

**Parameters**

-   `id` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Identifier of the translation to format
-   `args` **\[[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)]** Optional external arguments

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** 

# interactive

A Promise which resolves when the `Localization` instance has fetched
and parsed all localization resources in the user's first preferred
language (if available).

    localization.interactive.then(callback);
