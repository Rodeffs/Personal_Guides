Python environments are very useful in case you want to install different libraries and then export them all at once to work somewhere else

On Arch Linux you can install all the Python libraries via pacman, but they will be install globally and that may not be desired for various reasons

In the virtual environments everything is installed locally specifically for the selected environment. You can install libraries with *pip* plus you can use different Python and libraries versions in them

To make a virtual environment, execute:

```
python -m venv envname
```

where *envname* will be the name of the environment

Then, to activate it, do:

```
source envname/bin/activate
```

To deactivate it, simply type:

```
deactivate
```

It may be preferrable to export the filepath as a variable in your shell, like this:

```
export PYENV="envname/bin/activate"
```

so that you can just run:

```
source $PYENV
```
