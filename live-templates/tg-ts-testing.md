#### testing live templates:

 * ``test`` creates a test that will run with all other tests
 * ``testf`` creates a describe and it block to test a lib function
 * ``desc`` creates a describe block
 * ``it`` creates an it block
 * ``ita`` creates an async it block

The following are the same as above, but append ``.only`` to stop any other tests from running.
This allows you to run on your test file, or describe block or it block.
So when you run ``npm run mocha-w`` it will only run the blocks with ``.only``.
you can also attach ``.skip`` to skip a test.

 * ``testo``
 * ``testfo``
 * ``desco``
 * ``ito``
 * ``itao``
