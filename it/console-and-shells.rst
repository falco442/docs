Shells, Tasks & Console Tools
#############################

CakePHP non si propone solo come framework web, ma anche come framework per creare applicazioni per riga di comando.
Le applicazioni da riga di comando sono ideali per gestire una varietà di compiti in background come la manutenzione, 
e completare il lavoro al di fuori del ciclo request-response.
Le applicazioni da riga di comando di CakePHP ti permettono di riutilizzare le classi della tua applicazione dalla riga di comando.

CakePHP viene con un numero di applicazioni da riga di comando già definite.
Alcune di queste sono utilizzate in concertazione con le altre caratteristiche di CakePHP (come ACL o i18n),
ed altre sono per un utilizzo generale in modo da rendere più veloce il tuo lavoro.


.. _the-cakephp-console:

La console di CakePHP
===================

Questa sezione fornisce un'introduzione per la riga di comando di CakePHP.
Se hai mai avuto bisogno delle tue classi MVC di CakePHP in un cronjob o un altro script da riga di comando,
questa sezione è per te.

PHP fornisce un client CLI (Command Line Interface) che rende più fluido
l'interfacciarsi con il file system e le applicazioni.
La console CakePHP fornisce un framework per creare scripts da riga di comando.
La Console utilizza un'impostazione di tipo dispatcher per caricare una shell o un task, e fornisce i suoi parametri.


.. note::

    Una command-line (CLI) costruita da PHP è disponibile nel sistema se pensi di utilizzare la Console.

Prima di entrare nello spefico, assicuriamoci che possiamo eseguire la console di CakePHP.
Prima di tutto, devi chiamare una shell di sistema. Gli esempi mostrati in questa sezione saranno in bash,
ma la console di CakePHP è ugualmente compatibile con Windows.
Eseguiamo un programma di Console dalla bash.
Questo esempio assume che l'utente sia loggato nel prompt della bash e sia posizionato alla radice dell'applicazione CakePHP.

Le applicazioni CakePHP contengono una directory ``Console`` che contiene tutte le shell e i task per un'applicazione.
Viene insieme ad un eseguibile::


    $ cd /percorso/in/cakephp/app
    $ Console/cake

Spesso è saggio aggiungere l'eseguibile del core di cake al tuo percorso di sistema,
così che tu possa usare il comando cake dovunque. Questo torna utile il momento che stai creando nuovi progetti.
Guarda :ref:`adding-cake-to-your-path` per sapere come rendere ``cake`` disponibile come comando di sistema.

Eseguendo la Console senza argomenti produce il seguente messaggio di help::

    Welcome to CakePHP v2.0.0 Console
    ---------------------------------------------------------------
    App : app
    Path: /path/to/cakephp/app/
    ---------------------------------------------------------------
    Current Paths:

     -app: app
     -working: /path/to/cakephp/app
     -root: /path/to/cakephp/
     -core: /path/to/cakephp/core

    Changing Paths:

    your working path should be the same as your application path
    to change your path use the '-app' param.
    Example: -app relative/path/to/cakephp/app or -app /absolute/path/to/cakephp/app

    Available Shells:

     acl [CORE]                              i18n [CORE]
     api [CORE]                              import [app]
     bake [CORE]                             schema [CORE]
     command_list [CORE]                     testsuite [CORE]
     console [CORE]                          upgrade [CORE]

    To run a command, type 'cake shell_name [args]'
    To get help on a specific command, type 'cake shell_name help'

La prima informazione stampata si riferisce ai percorsi.
Questo è particolarmente utile se stai eseguendo laa console da punti differenti del filesystem.

Molti utenti aggiungono la console di CakePHP al loro percorso di sistema in modo che possa essere acceduta facilmente.
Stampare le informazioni di root, app, core e percorsi ti permette di vedere dove la console sta lavorando per applicare cambiamenti.
Per cambiare la directory app con la quale vuoi lavorare, puoi fornire il suo percorso come primo argomento al comando cake.
Questo prossimo esempio mostra come specificare una cartella app al comando cake,
assuento che tu abbia già aggiunto la cartella Console al tuo ``PATH``::


    $ cake -app /path/to/cakephp/app

Il percorso fornito può essere relativo rispetto alla directory corrente
o fornito come percorso assoluto.

.. _adding-cake-to-your-path:

Aggiungere cake al tuo percorso di sistema
-------------------------------

Se sei in un sistema \*nix (linux, MacOSX) i passi seguenti ti permetteranno di aggiungere
l'eseguibile cake al tuo percorso di sistema.


#. Posizionati nella directory di installazione di CakePHP, e dove sono gli eseguibili. Per esempio
   ``/Users/mark/cakephp/lib/Cake/Console/cake``
#. Modifica il file ``.bashrc`` o ``.bash_profile`` nella tua directory home, e aggiungi le righe seguenti::

    export PATH="$PATH:/Users/mark/cakephp/lib/Cake/Console"

#. Ricarica la configurazione della bash o apri un nuovo terminale, ed il comando ``cake`` dovrebbe ora funzionare da un qualsiasi percorso.

Se sei sotto sistema operativo Windows Vista o 7, dovresti seguire i seguenti passi.


#. Posizionati dove si trova la directory di installazione di CakePHP, e dove sono gli eseguibili. Per esempio
   ``C:\xampp\htdocs\cakephp\lib\Cake\Console``   

#. Apri le Proprietà di Sistema da My Computer. Mediante la shortcut Tasto Windows + Pause o Tasto Windows + Break. o, dal Desktop, fai click col destro su My Computer, clicca Proprietà e poi clicca il link Impostazioni di Sistema Avanzate dalla colonna di sinistra
#. Passa al tab Avanzate e clicca sul pulsante Variabili di Ambiente
#. Nella porzione Variabili di Sistema, raggiungi la variabile Percorso e facci doppio click per modificarla
#. Aggiungi il percorso di installazione ``cake`` seguito da due punti. Un risultato di esempio::

    %SystemRoot%\system32;%SystemRoot%;C:\xampp\htdocs\cakephp\lib\Cake\Console;

#. Clicca OK e il comando ``cake`` dovrebbe essere disponibile da qualsiasi percorso.

Creare una shell
================

Creiamo ora una shell da usare nella Console. Per questo esempio,
creeremo una semplice shell Helllo World. Nella tua app directory ``Console/Command`` crea un file ``HelloShell.php``. Inserisci il seguente codice in esso::


    class HelloShell extends AppShell {
        public function main() {
            $this->out('Hello world.');
        }
    }

Le convenzioni per le classi di shell sono che il nome della classe dovrebbe corrispondere al nome del file, con il suffisso Shell.
Nella nostra scell abbiamo creato un metodo ``main()``.
Questo metodo è chiamato quando una shell è chiamata senza parametri addizionali.
Aggiungeremo alcuni comandi a breve, ma per ora semplicemente limitiamoci ad eseguire la shell.
Dalla directory della tua applicazione, esegui::

    Console/cake hello

Vedrai il seguente output::

    Welcome to CakePHP v2.0.0 Console
    ---------------------------------------------------------------
    App : app
    Path: /Users/markstory/Sites/cake_dev/app/
    ---------------------------------------------------------------
    Hello world.

Come prima menzionato, il metodo ``main()`` nella shell è un metodo speciale chiamato
ogniqualvolta non ci sono altri comandi o argomenti dati ad una shell.
Avrai anche notato che HelloShell sta estendendo la classe ``AppShell``.
Esattamente come per :ref:`app-controller`, AppShell ti fornisce già una classe base
per contenere tutte le funzionalità o le logiche comuni. Puoi definire una AppShell, creando
``app/Console/Command/AppShell.php``. Se non ne hai una, CakePHP utilizzerà quella di sistema.
Dal momento che il nostro metodo main non era molto interessante, aggiungiamo un altro comando
che faccia qualcosa::


    class HelloShell extends AppShell {
        public function main() {
            $this->out('Hello world.');
        }

        public function hey_there() {
            $this->out('Hey there ' . $this->args[0]);
        }
    }

Dopo aver salvato questo file dovresti poter eseguire ``Console/cake hello hey_there your-name``
e vedere il tuo nome stampato a schermo. Nel nostro metodo ``hey_there`` abbiamo anche usato ``$this->args``;
queesta proprietà contiene un array di tutti gli argomenti posizionali forniti al comando.
Puoi anche usare switch o opzioni nelle tue applicazioni shell, queste sono disponibili nella proprietà
``$this->params``, e attraverso il metodo ``param()``, ma ci arriveremo a breve.

Quando utilizzi il metodo ``main()`` non puoi usare gli argomenti posizionali, o i parametri.
Questo perché il primo argomento posizionale, o la prima opzione, è interpretata come il nome di un comando.
Se vuoi utilizzare questi argomenti ed opzioni, dovresti usare altri nomi per i metodi, diversi da ``main``.


Usare i Modelli nelle tue shell
---------------------------

Ti capiterà spesso di aver bisogno di accedere alle logiche della tua applicazione nelle
utilità di shell; CakePHP rende questo super semplice. Impostando la proprietà
``$uses``, puoi definire un array di modelli ai quali vuoi avere accesso nella tua shell.
I modelli definiti sono caricati come proprietà allegate alla tua shell, proprio come
i Controllers prendono il modello ad essi associato::


    class UserShell extends AppShell {
        public $uses = array('User');

        public function show() {
            $user = $this->User->findByUsername($this->args[0]);
            $this->out(print_r($user, true));
        }
    }

La shell di sopra, prenderà un utente a partire dal suo nome utente, e stamperà le informazioni
immagazzinate nel database.


Shell tasks
===========

Verranno i tempi in cui, costruendo applicazioni per riga di comando più avanzate, vorrai
comporre le funzionalità in classi riutilizzabili che possano essere condivise da più shells.
I Task ti permettono di estrarre questi comandi in classi. Per esempio, il comando ``bake``
è chiamato in quasi tutti i task.
Puoi definire i task chiamati da una shell usando la proprietà ``$tasks``::


    class UserShell extends AppShell {
        public $tasks = array('Template');
    }

Poi usare i task dai plugin utilizzando la notazione standard :term:`plugin syntax`.
I Task sono depositati nella directory ``Console/Command/Task/`` chiamati come le loro classi.
Così, se stiamo per creare un Task di nome 'FileGenerator', creeremo il file
``Console/Command/Task/FileGeneratorTask.php``.

Ogni task deve implementare almeno un metodo ``execute()``. La ShellDispatcher,
chiamerà questo metodo quando il task è invocato. Un task di una shell appare come::


    class FileGeneratorTask extends Shell {
        public $uses = array('User');
        public function execute() {

        }
    }

Una shell può anche accedere alle proprietà del suo task, che rende i task ottimi per
costruire pezzi di codice riutilizzabili simili a to :doc:`/controllers/components`::

    // si troverà in Console/Command/SeaShell.php
    class SeaShell extends AppShell {
        public $tasks = array('Sound'); // found in Console/Command/Task/SoundTask.php
        public function main() {
            $this->Sound->execute();
        }
    }

Puoi accedere ai task anche direttamente dalla riga di comando::

    $ cake sea sound

.. note::

    Per accedere ai task direttamente dalla riga di comando,
    il task **deve** essere incluso nella proprietà ``$tasks`` della shell.
    Quindi, sappi che un metodo chiamato "sound" nella classe SeaShell
    sovrascrivverebbe l'abilità di accedere alla funzionalità Sound del task specificato
    nell'array $tasks.
    

Caricare i task al volo con TaskCollection
--------------------------------------------

Puoi caricare i task al volo utilizzando l'oggetto Task Collection, Puoi caricare i task
che non erano dichiarati in $tasks in questo modo::


    $Project = $this->Tasks->load('Project');

Caricherà e ritornerà un'istanza di ProjectTast. Puoi caricare i task dai plugin usando::


    $ProgressBar = $this->Tasks->load('ProgressBar.ProgressBar');

.. _invoking-other-shells-from-your-shell:

Invocare altre shell dalla tua shell
=====================================

Le shell non hanno più accesso diretto alla ShellDispatcher attraverso `$this->Dispatch`.
Ci sono molti casi dove vorrai invocare una shell da un'altra, però.
`Shell::dispatchShell()` di dà la possibilità di chiamare altre shell fornendo il `argv` perr la sottoshell.
Puoi fornire argomenti ed opzioni o come var args o come stringhe::


    // Come stringa
    $this->dispatchShell('schema create Blog --plugin Blog');

    // Come array
    $this->dispatchShell('schema', 'create', 'Blog', '--plugin', 'Blog');

L'esempio di sopra mostra come puoi chiamare la shell schema per creare lo schema per un plugin
all'interno di una shell di un tuo plugin.


.. _shell-output-level:

Livelli di output della Console
=====================

Shells often need different levels of verbosity. When running as cron jobs,
most output is un-necessary. And there are times when you are not interested in
everything that a shell has to say. You can use output levels to flag output
appropriately. The user of the shell, can then decide what level of detail
they are interested in by setting the correct flag when calling the shell.
:php:meth:`Shell::out()` supports 3 types of output by default.

* QUIET - Only absolutely important information should be marked for quiet output.
* NORMAL - The default level, and normal usage
* VERBOSE - Mark messages that may be too noisy for everyday use, but helpful
  for debugging as VERBOSE

You can mark output as follows::

    // would appear at all levels.
    $this->out('Quiet message', 1, Shell::QUIET);

    // would not appear when quiet output is toggled
    $this->out('normal message', 1, Shell::NORMAL);
    $this->out('loud message', 1, Shell::VERBOSE);

    // would only appear when verbose output is enabled.
    $this->out('extra message', 1, Shell::VERBOSE);

You can control the output level of shells, by using the ``--quiet`` and ``--verbose``
options. These options are added by default, and allow you to consistently control
output levels inside your CakePHP shells.

Styling output
==============

Styling output is done by including tags - just like HTML - in your output.
ConsoleOutput will replace these tags with the correct ansi code sequence, or
remove the tags if you are on a console that doesn't support ansi codes. There
are several built-in styles, and you can create more. The built-in ones are

* ``error`` Error messages. Red underlined text.
* ``warning`` Warning messages. Yellow text.
* ``info`` Informational messages. Cyan text.
* ``comment`` Additional text. Blue text.
* ``question`` Text that is a question, added automatically by shell.

You can create additional styles using `$this->stdout->styles()`. To declare a
new output style you could do::

    $this->stdout->styles('flashy', array('text' => 'magenta', 'blink' => true));

This would then allow you to use a ``<flashy>`` tag in your shell output, and if ansi
colours are enabled, the following would be rendered as blinking magenta text
``$this->out('<flashy>Whoooa</flashy> Something went wrong');``. When defining
styles you can use the following colours for the `text` and `background` attributes:

* black
* red
* green
* yellow
* blue
* magenta
* cyan
* white

You can also use the following options as boolean switches, setting them to a
truthy value enables them.

* bold
* underline
* blink
* reverse

Adding a style makes it available on all instances of ConsoleOutput as well,
so you don't have to redeclare styles for both stdout and stderr objects.

Turning off colouring
---------------------

Although colouring is pretty awesome, there may be times when you want to turn it off,
or force it on::

    $this->stdout->outputAs(ConsoleOutput::RAW);

The above will put the output object into raw output mode. In raw output mode,
no styling is done at all. There are three modes you can use.

* ``ConsoleOutput::RAW`` - Raw output, no styling or formatting will be done.
  This is a good mode to use if you are outputting XML or, want to debug why
  your styling isn't working.
* ``ConsoleOutput::PLAIN`` - Plain text output, known style tags will be stripped
  from the output.
* ``ConsoleOutput::COLOR`` - Output with color escape codes in place.

By default on \*nix systems ConsoleOutput objects default to colour output.
On Windows systems, plain output is the default unless the ``ANSICON`` environment
variable is present.

Configuring options and generating help
=======================================

.. php:class:: ConsoleOptionParser

Console option parsing in CakePHP has always been a little bit different
from everything else on the command line. In 2.0 ``ConsoleOptionParser``
helps provide a more familiar command line option and argument parser.

OptionParsers allow you to accomplish two goals at the same time.
First they allow you to define the options and arguments, separating
basic input validation and your code. Secondly, it allows you to provide
documentation, that is used to generate well formatted help file.

The console framework gets your shell's option parser by calling
``$this->getOptionParser()``. Overriding this method allows you to
configure the OptionParser to match the expected inputs of your shell.
You can also configure subcommand option parsers, which allow you to
have different option parsers for subcommands and tasks.
The ConsoleOptionParser implements a fluent interface and includes
methods for easily setting multiple options/arguments at once. ::

    public function getOptionParser() {
        $parser = parent::getOptionParser();
        //configure parser
        return $parser;
    }

Configuring an option parser with the fluent interface
------------------------------------------------------

All of the methods that configure an option parser can be chained,
allowing you to define an entire option parser in one series of method calls::

    public function getOptionParser() {
        $parser = parent::getOptionParser();
        $parser->addArgument('type', array(
            'help' => 'Either a full path or type of class.'
        ))->addArgument('className', array(
            'help' => 'A CakePHP core class name (e.g: Component, HtmlHelper).'
        ))->addOption('method', array(
            'short' => 'm',
            'help' => __('The specific method you want help on.')
        ))->description(__('Lookup doc block comments for classes in CakePHP.'));
        return $parser;
    }

The methods that allow chaining are:

- description()
- epilog()
- command()
- addArgument()
- addArguments()
- addOption()
- addOptions()
- addSubcommand()
- addSubcommands()

.. php:method:: description($text = null)

Gets or sets the description for the option parser. The description
displays above the argument and option information. By passing in
either an array or a string, you can set the value of the description.
Calling with no arguments will return the current value::

    // Set multiple lines at once
    $parser->description(array('line one', 'line two'));

    // read the current value
    $parser->description();

.. php:method:: epilog($text = null)

Gets or sets the epilog for the option parser. The epilog
is displayed after the argument and option information. By passing in
either an array or a string, you can set the value of the epilog.
Calling with no arguments will return the current value::

    // Set multiple lines at once
    $parser->epilog(array('line one', 'line two'));

    // read the current value
    $parser->epilog();

Adding arguments
----------------

.. php:method:: addArgument($name, $params = array())

Positional arguments are frequently used in command line tools,
and ``ConsoleOptionParser`` allows you to define positional
arguments as well as make them required. You can add arguments
one at a time with ``$parser->addArgument();`` or multiple at once
with ``$parser->addArguments();``::

    $parser->addArgument('model', array('help' => 'The model to bake'));

You can use the following options when creating an argument:

* ``help`` The help text to display for this argument.
* ``required`` Whether this parameter is required.
* ``index`` The index for the arg, if left undefined the argument will be put
   onto the end of the arguments. If you define the same index twice the
   first option will be overwritten.
* ``choices`` An array of valid choices for this argument. If left empty all
   values are valid. An exception will be raised when parse() encounters an
   invalid value.

Arguments that have been marked as required will throw an exception when
parsing the command if they have been omitted. So you don't have to
handle that in your shell.

.. php:method:: addArguments(array $args)

If you have an array with multiple arguments you can use ``$parser->addArguments()``
to add multiple arguments at once. ::

    $parser->addArguments(array(
        'node' => array('help' => 'The node to create', 'required' => true),
        'parent' => array('help' => 'The parent node', 'required' => true)
    ));

As with all the builder methods on ConsoleOptionParser, addArguments
can be used as part of a fluent method chain.

Validating arguments
--------------------

When creating positional arguments, you can use the ``required`` flag, to
indicate that an argument must be present when a shell is called.
Additionally you can use ``choices`` to force an argument to
be from a list of valid choices::

    $parser->addArgument('type', array(
        'help' => 'The type of node to interact with.',
        'required' => true,
        'choices' => array('aro', 'aco')
    ));

The above will create an argument that is required and has validation
on the input. If the argument is either missing, or has an incorrect
value an exception will be raised and the shell will be stopped.

Adding Options
--------------

.. php:method:: addOption($name, $options = array())

Options or flags are also frequently used in command line tools.
``ConsoleOptionParser`` supports creating options
with both verbose and short aliases, supplying defaults
and creating boolean switches. Options are created with either
``$parser->addOption()`` or ``$parser->addOptions()``. ::

    $parser->addOption('connection', array(
        'short' => 'c',
        'help' => 'connection',
        'default' => 'default',
    ));

The above would allow you to use either ``cake myshell --connection=other``,
``cake myshell --connection other``, or ``cake myshell -c other``
when invoking the shell. You can also create boolean switches, these switches do not
consume values, and their presence just enables them in the
parsed parameters. ::

    $parser->addOption('no-commit', array('boolean' => true));

With this option, when calling a shell like ``cake myshell --no-commit something``
the no-commit param would have a value of true, and 'something'
would be a treated as a positional argument.
The built-in ``--help``, ``--verbose``, and ``--quiet`` options
use this feature.

When creating options you can use the following options to
define the behavior of the option:

* ``short`` - The single letter variant for this option, leave undefined for none.
* ``help`` - Help text for this option. Used when generating help for the option.
* ``default`` - The default value for this option. If not defined the default will be true.
* ``boolean`` - The option uses no value, it's just a boolean switch.
  Defaults to false.
* ``choices`` An array of valid choices for this option. If left empty all
  values are valid. An exception will be raised when parse() encounters an invalid value.

.. php:method:: addOptions(array $options)

If you have an array with multiple options you can use ``$parser->addOptions()``
to add multiple options at once. ::

    $parser->addOptions(array(
        'node' => array('short' => 'n', 'help' => 'The node to create'),
        'parent' => array('short' => 'p', 'help' => 'The parent node')
    ));

As with all the builder methods on ConsoleOptionParser, addOptions is can be used
as part of a fluent method chain.

Validating options
------------------

Options can be provided with a set of choices much like positional arguments
can be. When an option has defined choices, those are the only valid choices
for an option. All other values will raise an ``InvalidArgumentException``::

    $parser->addOption('accept', array(
        'help' => 'What version to accept.',
        'choices' => array('working', 'theirs', 'mine')
    ));

Using boolean options
---------------------

Options can be defined as boolean options, which are useful when you need to create
some flag options. Like options with defaults, boolean options always include
themselves into the parsed parameters. When the flags are present they are set
to true, when they are absent false::

    $parser->addOption('verbose', array(
        'help' => 'Enable verbose output.',
        'boolean' => true
    ));

The following option would result in ``$this->params['verbose']`` always
being available. This lets you omit ``empty()`` or ``isset()``
checks for boolean flags::

    if ($this->params['verbose']) {
        // do something
    }

    // as of 2.7
    if ($this->param('verbose')) {
        // do something
    }

Since the boolean options are always defined as ``true`` or
``false`` you can omit additional check methods.

Adding subcommands
------------------

.. php:method:: addSubcommand($name, $options = array())

Console applications are often made of subcommands, and these subcommands
may require special option parsing and have their own help. A perfect
example of this is ``bake``. Bake is made of many separate tasks that all
have their own help and options. ``ConsoleOptionParser`` allows you to
define subcommands and provide command specific option parsers so the
shell knows how to parse commands for its tasks::

    $parser->addSubcommand('model', array(
        'help' => 'Bake a model',
        'parser' => $this->Model->getOptionParser()
    ));

The above is an example of how you could provide help and a specialized
option parser for a shell's task. By calling the Task's ``getOptionParser()``
we don't have to duplicate the option parser generation, or mix concerns
in our shell. Adding subcommands in this way has two advantages.
First it lets your shell easily document its subcommands in the
generated help, and it also allows easy access to the subcommand
help. With the above subcommand created you could call
``cake myshell --help`` and see the list of subcommands, and
also run ``cake myshell model --help`` to view the help for
just the model task.

When defining a subcommand you can use the following options:

* ``help`` - Help text for the subcommand.
* ``parser`` - A ConsoleOptionParser for the subcommand. This allows you
  to create method specific option parsers. When help is generated for a
  subcommand, if a parser is present it will be used. You can also
  supply the parser as an array that is compatible with
  :php:meth:`ConsoleOptionParser::buildFromArray()`

Adding subcommands can be done as part of a fluent method chain.

Building a ConsoleOptionParser from an array
--------------------------------------------

.. php:method:: buildFromArray($spec)

As previously mentioned, when creating subcommand option parsers,
you can define the parser spec as an array for that method. This can help
make building subcommand parsers easier, as everything is an array::

    $parser->addSubcommand('check', array(
        'help' => __('Check the permissions between an ACO and ARO.'),
        'parser' => array(
            'description' => array(
                __("Use this command to grant ACL permissions. Once executed, the "),
                __("ARO specified (and its children, if any) will have ALLOW access "),
                __("to the specified ACO action (and the ACO's children, if any).")
            ),
            'arguments' => array(
                'aro' => array('help' => __('ARO to check.'), 'required' => true),
                'aco' => array('help' => __('ACO to check.'), 'required' => true),
                'action' => array('help' => __('Action to check'))
            )
        )
    ));

Inside the parser spec, you can define keys for ``arguments``, ``options``,
``description`` and ``epilog``. You cannot define ``subcommands`` inside an
array style builder. The values for arguments, and options, should follow the
format that :php:func:`ConsoleOptionParser::addArguments()` and
:php:func:`ConsoleOptionParser::addOptions()` use. You can also use
buildFromArray on its own, to build an option parser::

    public function getOptionParser() {
        return ConsoleOptionParser::buildFromArray(array(
            'description' => array(
                __("Use this command to grant ACL permissions. Once executed, the "),
                __("ARO specified (and its children, if any) will have ALLOW access "),
                __("to the specified ACO action (and the ACO's children, if any).")
            ),
            'arguments' => array(
                'aro' => array('help' => __('ARO to check.'), 'required' => true),
                'aco' => array('help' => __('ACO to check.'), 'required' => true),
                'action' => array('help' => __('Action to check'))
            )
        ));
    }

Getting help from shells
------------------------

With the addition of ConsoleOptionParser getting help from shells is done
in a consistent and uniform way. By using the ``--help`` or -``h`` option you
can view the help for any core shell, and any shell that implements a ConsoleOptionParser::

    cake bake --help
    cake bake -h

Would both generate the help for bake. If the shell supports subcommands
you can get help for those in a similar fashion::

    cake bake model --help
    cake bake model -h

This would get you the help specific to bake's model task.

Getting help as XML
-------------------

When building automated tools or development tools that need to interact
with CakePHP shells, it's nice to have help available in a machine parse-able
format. The ConsoleOptionParser can provide help in xml by setting an
additional argument::

    cake bake --help xml
    cake bake -h xml

The above would return an XML document with the generated help, options,
arguments and subcommands for the selected shell. A sample XML document
would look like:

.. code-block:: xml

    <?xml version="1.0"?>
    <shell>
        <command>bake fixture</command>
        <description>Generate fixtures for use with the test suite. You can use
            `bake fixture all` to bake all fixtures.</description>
        <epilog>
            Omitting all arguments and options will enter into an interactive
            mode.
        </epilog>
        <subcommands/>
        <options>
            <option name="--help" short="-h" boolean="1">
                <default/>
                <choices/>
            </option>
            <option name="--verbose" short="-v" boolean="1">
                <default/>
                <choices/>
            </option>
            <option name="--quiet" short="-q" boolean="1">
                <default/>
                <choices/>
            </option>
            <option name="--count" short="-n" boolean="">
                <default>10</default>
                <choices/>
            </option>
            <option name="--connection" short="-c" boolean="">
                <default>default</default>
                <choices/>
            </option>
            <option name="--plugin" short="-p" boolean="">
                <default/>
                <choices/>
            </option>
            <option name="--records" short="-r" boolean="1">
                <default/>
                <choices/>
            </option>
        </options>
        <arguments>
            <argument name="name" help="Name of the fixture to bake.
                Can use Plugin.name to bake plugin fixtures." required="">
                <choices/>
            </argument>
        </arguments>
    </shell>

Routing in shells / CLI
=======================

In command-line interface (CLI), specifically your shells and tasks, ``env('HTTP_HOST')`` and
other webbrowser specific environment variables are not set.

If you generate reports or send emails that make use of ``Router::url()`` those will contain
the default host ``http://localhost/``  and thus resulting in invalid URLs. In this case you need to
specify the domain manually.
You can do that using the Configure value ``App.fullBaseURL`` from your bootstrap or config, for example.

For sending emails, you should provide CakeEmail class with the host you want to send the email with::

    $Email = new CakeEmail();
    $Email->domain('www.example.org');

This asserts that the generated message IDs are valid and fit to the domain the emails are sent from.

Shell API
=========

.. php:class:: AppShell

    AppShell can be used as a base class for all your shells. It should extend
    :php:class:`Shell`, and be located in ``Console/Command/AppShell.php``

.. php:class:: Shell($stdout = null, $stderr = null, $stdin = null)

    Shell is the base class for all shells, and provides a number of functions for
    interacting with user input, outputting text a generating errors.

.. php:attr:: tasks

    An array of tasks you want loaded for this shell/task.

.. php:attr:: uses

    An array of models that should be loaded for this shell/task.

.. php:method:: clear()

    Clears the current output being displayed.

.. php:method:: param($name)

    Get the value of an option/parameter. Will return null if the parameter does
    not exist.

    .. versionadded:: 2.7

.. php:method:: createFile($path, $contents)

    :param string $path: Absolute path to the file you want to create.
    :param string $contents: Contents to put in the file.

    Creates a file at a given path. If the Shell is interactive, a warning will be
    generated, and the user asked if they want to overwrite the file if it already exists.
    If the shell's interactive property is false, no question will be asked and the file
    will simply be overwritten.

.. php:method:: dispatchShell()

    Dispatch a command to another Shell. Similar to
    :php:meth:`Controller::requestAction()` but intended for running shells
    from other shells.

    See :ref:`invoking-other-shells-from-your-shell`.

.. php:method:: err($message = null, $newlines = 1)

    :param string $method: The message to print.
    :param integer $newlines: The number of newlines to follow the message.

    Outputs a method to ``stderr``, works similar to :php:meth:`Shell::out()`

.. php:method:: error($title, $message = null)

    :param string $title: Title of the error
    :param string $message: An optional error message

    Displays a formatted error message and exits the application with status
    code 1

.. php:method:: getOptionParser()

    Should return a :php:class:`ConsoleOptionParser` object, with any
    sub-parsers for the shell.

.. php:method:: hasMethod($name)

    Check to see if this shell has a callable method by the given name.

.. php:method:: hasTask($task)

    Check to see if this shell has a task with the provided name.

.. php:method:: hr($newlines = 0, $width = 63)

    :param int $newlines: The number of newlines to precede and follow the line.
    :param int $width: The width of the line to draw.

    Create a horizontal line preceded and followed by a number of newlines.

.. php:method:: in($prompt, $options = null, $default = null)

    :param string $prompt: The prompt to display to the user.
    :param array $options: An array of valid choices the user can pick from.
       Picking an invalid option will force the user to choose again.
    :param string $default: The default option if there is one.

    This method helps you interact with the user, and create interactive shells.
    It will return the users answer to the prompt, and allows you to provide a
    list of valid options the user can choose from::

        $selection = $this->in('Red or Green?', array('R', 'G'), 'R');

    The selection validation is case-insensitive.

.. php:method:: initialize()

    Initializes the Shell acts as constructor for subclasses allows
    configuration of tasks prior to shell execution.

.. php:method:: loadTasks()

    Loads tasks defined in public :php:attr:`Shell::$tasks`

.. php:method:: nl($multiplier = 1)

    :param int $multiplier: Number of times the linefeed sequence should be repeated

    Returns a number of linefeed sequences.

.. php:method:: out($message = null, $newlines = 1, $level = Shell::NORMAL)

    :param string $message: The message to print.
    :param integer $newlines: The number of newlines to follow the message.
    :param integer $level: The highest :ref:`shell-output-level` this message
        should display at.

    The primary method for generating output to the user. By using levels, you
    can limit how verbose a shell is. out() also allows you to use colour formatting
    tags, which will enable coloured output on systems that support it. There are
    several built-in styles for colouring text, and you can define your own.

    * ``error`` Error messages.
    * ``warning`` Warning messages.
    * ``info`` Informational messages.
    * ``comment`` Additional text.
    * ``question`` Magenta text used for user prompts

    By formatting messages with style tags you can display styled output::

        $this->out(
            '<warning>This will remove data from the filesystems.</warning>'
        );

    By default on \*nix systems ConsoleOutput objects default to colour output.
    On Windows systems, plain output is the default unless the ``ANSICON`` environment
    variable is present.

.. php:method:: overwrite($message = null, $newlines = 1, $size = null)

    :param string $message: The message to print.
    :param integer $newlines: The number of newlines to follow the message.
    :param integer $size: The number of bytes to overwrite

    A useful method to generate progress bars or to avoid outputting too many lines.

    Warning: You cannot overwrite text that contains newlines.

    .. versionadded:: 2.6

.. php:method:: runCommand($command, $argv)

    Runs the Shell with the provided argv.

    Delegates calls to Tasks and resolves methods inside the class. Commands
    are looked up with the following order:

    - Method on the shell.
    - Matching task name.
    - main() method.

    If a shell implements a main() method, all missing method calls will be
    sent to main() with the original method name in the argv.

.. php:method:: shortPath($file)

    Makes absolute file path easier to read.

.. php:method:: startup()

    Starts up the Shell and displays the welcome message. Allows for checking
    and configuring prior to command or main execution.

    Override this method if you want to remove the welcome information, or
    otherwise modify the pre-command flow.

.. php:method:: wrapText($text, $options = array())

    Wrap a block of text. Allows you to set the width, and indenting on a
    block of text.

    :param string $text: The text to format
    :param array $options:

        * ``width`` The width to wrap to. Defaults to 72
        * ``wordWrap`` Only wrap on words breaks (spaces) Defaults to true.
        * ``indent`` Indent the text with the string provided. Defaults to null.

More topics
===========

.. toctree::
    :maxdepth: 1

    console-and-shells/helpers
    console-and-shells/cron-jobs
    console-and-shells/completion-shell
    console-and-shells/code-generation-with-bake
    console-and-shells/schema-management-and-migrations
    console-and-shells/i18n-shell
    console-and-shells/acl-shell
    console-and-shells/testsuite-shell
    console-and-shells/upgrade-shell


.. meta::
    :title lang=en: Shells, Tasks & Console Tools
    :keywords lang=en: shell scripts,system shell,application classes,background tasks,line script,cron job,request response,system path,acl,new projects,shells,specifics,parameters,i18n,cakephp,directory,maintenance,ideal,applications,mvc
