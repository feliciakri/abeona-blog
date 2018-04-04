---
title: "Golang Database and CRUD Setup"
date: 2018-03-21T13:10:56+07:00
author: Wibisana Bramawidya
tags: ["Wibisana", "Golang", "Database", "TDD", "Sprint 1"]
---

> These past two weeks have been rough, but I will come through.

## The Past Two Weeks
I can't make up any excuses, these past two weeks haven't been the most productive. I've been slacking with my tasks, and repeatedly missed deadlines. During this period, my group has had 4 sprint meetings, every one of which I have only attended through a video call due to my inability to attend. The schedule for the meetings conflicted with my internship schedule, since I had to be at the office at those times. However, I still make time to join the meeting, even though only through video call, so that I won't miss out on the task division. On the flip side, Pak Adin made a vary much welcome change to the Git flow, one that I have been asking since week 1 (*not directly to him of course*), which is now we can pull a new branch for each task in all User Stories. **HALELUYA!!!**

## Holdups
For the most part, the past two weeks have been very hectic for me. Multiple courses have published tasks after tasks, sucking up a lot of my time, not to mention the workload from my Internship has increased since one of the interns finished up not long after my last post. This was the **main** thing taking up my time from working on this project, but not the only one. I won't go into much detail, but suffice it to say, it was my fault for making the poor decision to schedule my internship 3 months into the semester.

Another holdup is the fact that this is not the only course I am taking this semester. **I KNOW**, everyone working on this team has the same problem, *bla bla bla*. But since my internship is taking a fixed amount from my time every week, the extra work from other courses is doubly harsher for my time. As I said, this is a problem of my own making, but it is a problem nonetheless.

Lastly, the largest holdup is the fact that reviews and communication in my team takes a **REALLY** long time. I don't know if this is because my experience during my internship was working in a fast paced environment, where deadlines are always looming, so everyone usually responds immediately, and reviews come in less than an hour after my request, but the rate at which the team responds and reviews seems rather slow for me. Out of the 5 team members I am currently working with, 2 of them responds quite often and at a not crawling pace. I won't say anything about the others. I guess this may be me projecting my overly high expectations on them, or me just being a nuisance to the others trying to work on their parts, yeah, probably the latter.

## Actual Developments
Because of the holdups *see above*, I haven't made much progress, sadly :(. And even the progress that I have made, was in the general code, not the code specific to my tasks. The progress I have made this week is mostly just trying to figure out the flow of the back end. From how to access the Arango database, to reading the environment configurations, to the conventions for the error codes.

#### Environment Variable Fetching
Let me discuss the thing I have done this week one by one. The first and most important, I think is setting up the environment decoder `config.go` file. This file will be crucial for using settings in future development in Golang. As of now, the settings that should be stored *though it has a default value* is the `ARANGO_HOST`, `ARANGO_USERNAME`, `ARANGO_PASSWORD`, `ARANGO_DATABASE`. These variables are used to access the ArangoDB. These value will then be stored in the `Config` structure as such.

```Go
type Config struct {
	Arango struct {
		Host     string `env:"ARANGO_HOST,default=http://arangodb:8529"`
		Username string `env:"ARANGO_USERNAME,default=admin"`
		Password string `env:"ARANGO_PASSWORD,default=admin"`
		Database string `env:"ARANGO_DATABASE,default=admin"`
	}
}
```

 Thankfully, rather than coding them in to the structure one by one using `os.GetEnv`, I am using a package made by github username joeshaw [envdecode](https://github.com/joeshaw/envdecode). This package is extremely easy to use and gives a fair amount of flexibility when filling up the structure, letting it have nested values, rather than most other decoders, which do not.

#### ArangoDB Connection
The other significant progress that I have made is one that has to do with the ArangoDB. Using this code, just by calling a single function, the program can then connect to the ArangoDB, and directly query to the database. Functionally, this function (lol) does this in four steps:

- read the environment variables,
- create a connection to the database,
- create a client from the connection, and
- get the database from the client

These four steps can each produce an error by itself, so creating the test for them was a pain. In essence, a mock function needs to be created for each of them, then the `test case` has to patch each function so the external functions can produce mock errors. I this case, any errors produced by these functions are just forwarded to the caller function, so we only need to test mock fail as a generic mocker failure.

```Go
func mockHTTPNewConnectionFail(config http.ConnectionConfig) (driver.Connection, error) {
	return nil, driver.ArangoError(driver.ArangoError{HasError: true, Code: 404, ErrorNum: -1, ErrorMessage: "this is a mock failure"})
}

func mockDriverNewClientFail(config driver.ClientConfig) (driver.Client, error) {
	return nil, driver.ArangoError(driver.ArangoError{HasError: true, Code: 404, ErrorNum: -1, ErrorMessage: "this is a mock failure"})
}

func mockCoreGetEnvFail() (*core.Config, error) {
	return nil, errors.New("this is a mock error")
}
```

```Go
		"Bad Connection": GetDatabaseFib{
			PatchHTTPNewConnection: mockHTTPNewConnectionFail,
			DatabaseName:           "abeona",
			CollectionName:         "User",
			ExpectedDBError:        driver.ArangoError(driver.ArangoError{HasError: true, Code: 404, ErrorNum: -1, ErrorMessage: "this is a mock failure"}),
		},
		"Bad Client": GetDatabaseFib{
			PatchDriverNewClient: mockDriverNewClientFail,
			DatabaseName:         "abeona",
			CollectionName:       "User",
			ExpectedDBError:      driver.ArangoError(driver.ArangoError{HasError: true, Code: 404, ErrorNum: -1, ErrorMessage: "this is a mock failure"}),
		},
		"Env Error": GetDatabaseFib{
			PatchCoreGetEnv: mockCoreGetEnvFail,
			DatabaseName:    "abeona",
			CollectionName:  "User",
			ExpectedDBError: errors.New("this is a mock error"),
        },
```

> These tests use the same conventions as the one used in other tests (as mentioned in my last post)

```Go
func GetDatabaseTestHelp(t *testing.T, tc string, fib GetDatabaseFib) {
	// monkeypatch
	if fib.PatchHTTPNewConnection != nil {
		httpNewConnection = fib.PatchHTTPNewConnection
	}
	if fib.PatchDriverNewClient != nil {
		driverNewClient = fib.PatchDriverNewClient
	}
	if fib.PatchCoreGetEnv != nil {
		coreGetEnv = fib.PatchCoreGetEnv
	}

	// revert monkeypatch
	defer func() {
		httpNewConnection = http.NewConnection
		driverNewClient = driver.NewClient
		coreGetEnv = core.GetEnv
	}()

    ...
}
```

#### Error Codes
Lastly, the minute amount of progress I did was on error codes. On my last *individual review*, Pak Adin told me that error handling should be done with error codes, not just the plain error messages. So following his advice, I finally made a package to generate the error messages, although the implementation for each error message has to wait until the APIs and conventions are finalized. The implementations I made were extremely rudimentary. Basically i just made constants containing the Code and information about the error,

```Go
const (
	ErrSampleCode = 1
	ErrSampleStr  = "Failed to do something"
)
```

then just called these variables when invoking the specified error.

```Go
func SampleErr() error {
	return fmt.Errorf("%04d : %s", ErrSampleCode, ErrSampleStr)
}
```

Easy enough. We'll just see how this error scales, hopefully well.

#### Linter Cleanup
>This is not so much as development, as is best practice.

Since the beginning of this development, I haven't paid much attention to the linter warnings. And when I finally ran it for the first time last week, I realized the many warnings that were present, but even then I ignored them. Since then, every push I have done has generated a lot of warning, although it is not fatal or flawed in any way, it's still annoying to see warnings for every change I made. So yesterday, I finally decided to clean up the tens of lines of warnings and commit the all as one. As I said, not really development, neither significant, but still.

## WIP
Other than those, I haven't made much progress at all. The thing I am currently working on is creating a CRUD endpoint for products and merchants, I have been working on this section for a whole week, interrupted by the other things mentioned above, along with the holdups. All I have accomplished for this is just creating stubs and `structures` for the CRUD process.

Yeah, sadly my performance these past two weeks have been abysmal. Nothing I can say can fix that, and I can only try to be better for next week. So here's to hoping :wine:, and see you whenever I write another one of these.
