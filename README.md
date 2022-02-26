# Recursion error with PDB++ installed

To reproduce:

Uninstall PDB++ if installed:

```
pip uninstall pdbpp
```

```
jupyter notebook
```

Run the notebook.  Output is as expected, test failure, but no error.

Shut down Jupyter completely.

```
# Install into namespace used by Jupyter kernel.
pip install pdbpp
```

```
jupyter notebook
```

Run the notebook.  Output shows:

```
---------------------------------------------------------------------------
RecursionError                            Traceback (most recent call last)
<ipython-input-2-b54ec3250167> in <module>
----> 1 grader.check("q0")

~/Library/Python/3.8/lib/python/site-packages/otter/check/utils.py in run_function(self, *args, **kwargs)
    129                 except Exception as e:
    130                     self._log_event(event_type, success=False, error=e)
--> 131                     raise e
    132                 else:
    133                     self._log_event(event_type, results=results, question=question, shelve_env=shelve_env)

~/Library/Python/3.8/lib/python/site-packages/otter/check/utils.py in run_function(self, *args, **kwargs)
    122                 try:
    123                     if event_type == EventType.CHECK:
--> 124                         question, results, shelve_env = f(self, *args, **kwargs)
    125                     else:
    126                         results = f(self, *args, **kwargs)

~/Library/Python/3.8/lib/python/site-packages/otter/check/notebook.py in check(self, question, global_env)
    177 
    178         # run the check
--> 179         result = check(test_path, test_name, global_env)
    180 
    181         return question, result, global_env

~/Library/Python/3.8/lib/python/site-packages/otter/execute/__init__.py in check(nb_or_test_path, test_name, global_env)
     53         global_env = inspect.currentframe().f_back.f_globals
     54 
---> 55     test.run(global_env)
     56 
     57     return test

~/Library/Python/3.8/lib/python/site-packages/otter/test_files/ok_test.py in run(self, global_environment)
     94         """
     95         for i, test_case in enumerate(self.test_cases):
---> 96             passed, result = run_doctest(self.name + ' ' + str(i), test_case.body, global_environment)
     97             if passed:
     98                 result = 'Test case passed!'

~/Library/Python/3.8/lib/python/site-packages/otter/test_files/ok_test.py in run_doctest(name, doctest_string, global_environment)
     47     runresults = io.StringIO()
     48     with redirect_stdout(runresults), redirect_stderr(runresults), hide_outputs():
---> 49         doctestrunner.run(test, clear_globs=False)
     50     with open(os.devnull, 'w') as f, redirect_stderr(f), redirect_stdout(f):
     51         result = doctestrunner.summarize(verbose=True)

/usr/local/Cellar/python@3.8/3.8.12_1/Frameworks/Python.framework/Versions/3.8/lib/python3.8/doctest.py in run(self, test, compileflags, out, clear_globs)
   1467         save_trace = sys.gettrace()
   1468         save_set_trace = pdb.set_trace
-> 1469         self.debugger = _OutputRedirectingPdb(save_stdout)
   1470         self.debugger.reset()
   1471         pdb.set_trace = self.debugger.set_trace

/usr/local/Cellar/python@3.8/3.8.12_1/Frameworks/Python.framework/Versions/3.8/lib/python3.8/doctest.py in __init__(self, out)
    361         self.__debugger_used = False
    362         # do not play signal games in the pdb
--> 363         pdb.Pdb.__init__(self, stdout=out, nosigint=True)
    364         # still use input() to get user input
    365         self.use_rawinput = 1

~/Library/Python/3.8/lib/python/site-packages/IPython/core/debugger.py in __init__(self, color_scheme, completekey, stdin, stdout, context, **kwargs)
    225 
    226         # `kwargs` ensures full compatibility with stdlib's `pdb.Pdb`.
--> 227         OldPdb.__init__(self, completekey, stdin, stdout, **kwargs)
    228 
    229         # IPython changes...

~/Library/Python/3.8/lib/python/site-packages/_pdbpp_path_hack/pdb.py in __init__(self, *args, **kwds)
    167         kwargs = self.config.default_pdb_kwargs.copy()
    168         kwargs.update(**kwds)
--> 169         super(Pdb, self).__init__(*args, **kwargs)
    170         self.prompt = self.config.prompt
    171         self.display_list = {}  # frame --> (name --> last seen value)

... last 2 frames repeated, from the frame below ...

~/Library/Python/3.8/lib/python/site-packages/IPython/core/debugger.py in __init__(self, color_scheme, completekey, stdin, stdout, context, **kwargs)
    225 
    226         # `kwargs` ensures full compatibility with stdlib's `pdb.Pdb`.
--> 227         OldPdb.__init__(self, completekey, stdin, stdout, **kwargs)
    228 
    229         # IPython changes...

RecursionError: maximum recursion depth exceeded
```
