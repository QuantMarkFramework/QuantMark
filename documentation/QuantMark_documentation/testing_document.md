# Testing

Test are conducted with Pytest and Pytest-Django. 

### Unit tests:

*Note: at the writing of this subheading the unit tests comprises of a single file, 'qleader/tests/test_helpers.py', and are very bare bones. The instructions given wil lprobably change as the testing and test-writing workflows are developed further.*

Required packages for unit testing are listed in the document 'requirements-testing.txt'. They can be installed separately using
```
pip install -r requirements-testing.txt
```
but they are also installed when building the project (use sudo, if needed):
```
docker-compose build
```
After the required packages are installed, the unit tests can be run with
```
docker-compose run qleader-web py.test
```

## What was tested

## Reasons for testing
Since the application involves several intertwined objects and analyzing benchmark results on a separate backend, testing each "nook and cranny" makes it easier to ensure that inputs and outputs are valid, tasks get executed and so on.

## What is not tested
The UI-functionality is not tested, but during the development Google Lighthouse reports have been generated and changes have been made accordingly. Challenges with f.ex. bad layout have been "tested" in this way.

## Writing tests
Name of the test function must start with 'test'.
