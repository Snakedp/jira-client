Autoloading the JIRA library
============================

    <?php

    use Zend\Loader\StandardAutoloader;

    // Modify accordingly.
    $path_to_lib = '/path/to/downloaded/library';

    // Use any PSR-0 compliant autloader, such as Zend Framework's autoloader.
    $loader = new StandardAutoloader();
    $loader->registerNamespace('Jira', $path_to_lib . '/src/Jira');
    $loader->register();

    ?>


Authenticating against JIRA
===========================

    <?php

    use Jira\Client as JiraClient;

    // Modify accordingly, note that in some installations the JIRA instance is
    // in the document root and not in the "jira" subdirectory.
    $host = 'http://localhost:8090/jira';
    $username = 'my.username';
    $password = 'my.password';

    try {
        $jira = new JiraClient($host);
        $jira->login($username, $password);
    } catch (Exception $e) {
        // Something went wrong.
    }

    ?>


Fetching an issue
=================

    <?php

    try {
        $issue = $jira->issue('AB-123')->get();
    } catch (Exception $e) {
        // Something went wrong.
    }

    ?>

Creating an issue
=================

    <?php

    use Jira\Remote\Issue as RemoteIssue;

    $issue = new RemoteIssue();

    try {
        $issue
            ->setProject('AB')
            ->setType(1) // ID can be found via $jira->issueTypes()->get().
            ->setSummary('Issue created via the API')
            ->setDescription('This is a test issue created throug the API')
            ->create($jira);
    } catch (Exception $e) {
        // Something went wrong.
    }

    ?>