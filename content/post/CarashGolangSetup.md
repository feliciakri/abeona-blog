---
title: "Golang setup and testing"
date: 2018-03-07T12:03:47+07:00
author: Wibisana Bramawidya
tags: ["Wibisana", "Golang", "Setup", "TDD", "Sprint 1"]
---

> I am the Back End developer for Project Abeona. My name is Wibisana Bramawidya, and this is my story.

## Intro
This is my first week working on the class project for PPL (*Proyek Perangkat Lunak*). Because of that, my progress will not seem much, but that is because trying out different frameworks and conventions for the project forward has taken most of my time, since it will be crucial for the development going forward.

After trying out multiple frameworks and packages (this took almost a day on its own, **lol**), I ended up using the following:
- [Gin](https://github.com/gin-gonic/gin) for the service framework,
- [Gofight](https://github.com/appleboy/gofight) for testing,
- [Assert](github.com/stretchr/testify/assert) since apparently, Golang does not natively support assertion and
- [Custom coverage script](https://github.com/mlafeldt/chef-runner/blob/v0.7.0/script/coverage) to find the total coverage of the project

With these frameworks picked out, I can finally proceed to actually starting the development of Abeona's Back End.

## Routing and Handling with Gin
Picking out Gin for the framework seems to be the correct choice, *although picking out Golang turns out to produce more burdens that i hand previously imagined*. It provided easy logging for testing and debugging, and most of the requests and responses can be created very quickly and easily. Like the code below

```Go
adminGroup.PUT("/product/crud/:product_id", admin.HandleProductUpdate)
```

can easily set multiple endpoints with the URI itself containing the parameters. Creating a handler was also very simple. Even a code as short as this

```Go
func HandleIndex(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{
		"foo": "bar",
	})
}
```

can create working page, albeit static and bland (not that blandness really matters though, since this ARE API endpoints anyway)

## Testing Gin with Gofight
After setting up the routing and static handling, I then moved up to create the testing (`YES I KNOW`, TDD is supposed to start with the testing, **THEN** the implementation. But since this is my first time developing with TDD in mind using Golang, not to mention the frameworks I have never even heard of, I had to create dummies to check if the testing frameworks were even working as expected, and how to use them). Like picking out Gin, picking out Gofight also eases much of the work needed to test the implemented code.

After fiddling with multiple ways to test, I ended up settling on these conventions:
- For every tested `function`/`method`, there **MUST** be a `structure` to define its `MonkeyPatch` and expected `inputs` along with their respective `outputs`. It said `structure` looks like this,

```Go
    ProductCreateFib struct {
    	PatchJsonMarshal func(v interface{}) ([]byte, error) // MonkeyPatch for json.Marshal
    	ReqURI           string                              // Input URI
    	ReqBody          string                              // Input Body (Raw)
    	ResCode          int                                 // Output StatusCode
    	ResBody          string                              // Output Body (Raw with JSON format)
    }
```

- Every `test` should also have multiple `test cases` using the aforementioned `structure` to create uniform `test cases`, and lastly

```Go
	testFibs := map[string]ProductCreateFib{
		"Success": ProductCreateFib{
			ReqURI:  "/product/crud/1234",
			ReqBody: "{\"product_id\":1234,\"other\":\"foobar\"}",
			ResCode: http.StatusOK,
			ResBody: "{\"product_id\":1234}",
		},
		"Bad Request": ProductCreateFib{
			ReqURI:  "/product/crud/abcd",
			ReqBody: "{\"product_id\":1234,\"other\":\"foobar\"}",
			ResCode: http.StatusBadRequest,
			ResBody: "{\"err\":{\"Func\":\"ParseInt\",\"Num\":\"abcd\",\"Err\":{}}}",
		},
		"Bad Json Marshal": ProductCreateFib{
			PatchJsonMarshal: mockJsonMarshal_Fail,
			ReqURI:           "/product/crud/1234",
			ReqBody:          "{\"product_id\":1234,\"other\":\"foobar\"}",
			ResCode:          http.StatusBadRequest,
			ResBody:          "{\"err\":{}}",
		},
	}
```

- all the test should then be run in the same fashion as one another as such

```Go
    for _, fib := range testFibs {
        // monkeypatch
        if fib.PatchJsonMarshal != nil {
            jsonMarshal = fib.PatchJsonMarshal
        }

        r := gofight.New()
        r.POST(fib.ReqURI).
            SetBody(fib.ReqBody).
            Run(GinEngine(), func(res gofight.HTTPResponse, req gofight.HTTPRequest) {
                assert.Equal(t, fib.ResCode, res.Code)
                assert.Equal(t, fib.ResBody, res.Body.String())
            })

        // revert monkeypatch
        jsonMarshal = json.Marshal
    }
```

To test sending mock requests in Gin, it turns out a simple way is to just create a mock handler, which turns out to be much easier than creating and constructing a custom request

``` Go
func GinEngine() *gin.Engine {
	gin.SetMode(gin.TestMode)
	r := gin.New()

	r.POST("/product/crud/:product_id", HandleProductCreate)
	r.GET("/product/crud/:product_id", HandleProductRead)
	r.PUT("/product/crud/:product_id", HandleProductUpdate)
	r.DELETE("/product/crud/:product_id", HandleProductDelete)

	r.POST("/merchant/crud/:merchant_id", HandleMerchantCreate)
	r.GET("/merchant/crud/:merchant_id", HandleMerchantRead)
	r.PUT("/merchant/crud/:merchant_id", HandleMerchantUpdate)
	r.DELETE("/merchant/crud/:merchant_id", HandleMerchantDelete)

	return r
}
```

With the results above, I hope you can see what took so long. Creating a system to test everything uniformly and one that is easy to modify in the future took a long time for me (forgive me for being a complete noob at this).

Currently, all these `tests`, `test cases` and `structures` exists in one file, making it very long and cluttered. In the future, I would like to move the `structures` and `test cases` to a separate file to unclutter the code. But since time is running short, that has to be allocated to future weeks

## Coverage
After creating the tests for all the functions (there are a lot of them and making `tests` and `test cases` for every one of them is VERY tiring), the next problem to tackle is how to get the total coverage of this project.

Golang provides a unit test system that works very well and is genuinely easy to use. **BUT** there is one caveat, which is that it does **NOT** provide total coverage for the whole project. I have to confess, I was at a complete loss at what to do. So, after scouring the internet, I finally found this [https://github.com/mlafeldt/chef-runner/blob/v0.7.0/script/coverage](https://github.com/mlafeldt/chef-runner/blob/v0.7.0/script/coverage), a beautiful *bash script* created by username `mlafeldt` that can get the total coverage of the project. **HUZZAH** I finally did it.

After being able to find the total coverage of the project, all that was left is to integrate it with the CI, which only took the addition of 3 lines of code, whew.

## Conclusion
Well that was my first week (well not even a week after i think about it) of developing the Back End API of Abeona. The week went by without me making a lot of tangible progress, but I feel like the conventions decided on here will help the development go smoother, *hopefully*.

Thanks for reading, and I will see you whenever I feel like writing this again, probably when the next deadline of the blog post is coming up **XD**.
