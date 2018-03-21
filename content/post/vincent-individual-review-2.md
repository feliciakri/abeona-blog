---
title: "Vincent Individual Review 2"
date: 2018-03-21T10:51:24+07:00
author: Vincent
tags: ["Vincent", "Agile", "TDD", "Product Page Endpoint", "Sprint 1", "Week 7"]
---

Pada sprint 1 minggu ketiga, penulis mempelajari mengenai *Software Development: Agile* dan *Test Driven Development*. Kedua hal ini diterapkan pada perkuliahan Proyek Perangkat Lunak.

## Software Development: Agile
*Agile software development* adalah metode pengembangan perangkat lunak dimana pengembangannya dalam jangka pendek dan memerlukan respon yang cepat jika terdapat perubahan.

*Manifesto* yang terdapat pada agile software development, antara lain:
1. Individual dan interaksi lebih penting daripada proses dan alatnya
2. *Software* yang bekerja dengan baik lebih penting daripada dokumentasi yang komprehensif
3. Kolaborasi dengan pelanggan lebih penting daripada negosiasi kontrak
4. Menanggapi perubahan lebih penting daripada mengikuti rencana

12 *principle* yang terkandung dalam *agile software development*, antara lain:
1. Our highest priority is to satisfy the customer through early and continuous delivery of valuable software.
2. Welcome changing requirements, even late in development. Agile processes harness change for the customer's competitive advantage.
3. Deliver working software frequently, from a couple of weeks to a couple of months, with a preference to the shorter timescale.
4. Business people and developers must work together daily throughout the project.
5. Build projects around motivated individuals. Give them the environment and support they need, and trust them to get the job done.
6. The most efficient and effective method of conveying information to and within a development team is face-to-face conversation.
7. Working software is the primary measure of progress.
8. Agile processes promote sustainable development. The sponsors, developers, and users should be able to maintain a constant pace indefinitely.
9. Continuous attention to technical excellence and good design enhances agility.
10. Simplicity--the art of maximizing the amount of work not done--is essential.
11. The best architectures, requirements, and designs emerge from self-organizing teams.
12. At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly.

## Test Driven Development
Menurut penulis, *Test Driven Development* adalah gaya programming dimana aktivitasnya dibagi menjadi 3 bagian, yaitu *coding*, *testing* dan *desain*

Tahap - tahap untuk melakukan *Test Driven Development*, antara lain:
- Menulis *unit test* untuk menjelaskan program
- Menjalankan test dimana akan gagal karena program tidak memiliki fitur tersebut
- Mengimplementasikan fitur sehingga lolos *unit test*
- Melakukan *refactor* kode hingga sudah sesuai yang diinginkan
- Mengulangi tahap - tahap di atas hingga desain dari kode sudah sesuai dan lolos unit test

Selain itu, penulis juga mengerjakan task Backend, yaitu membuat endpoint untuk halaman *product*. 

## Product Page Endpoint 
Pada task ini, penulis mendaftarkan 2 endpoint pada router di backend yaitu endpoint untuk mengambil *product* dan *merchant* dengan menggunakan method GET. 
```yaml
mobileGroup.GET("/product", abeona.HandleProductView)
mobileGroup.GET("/merchant", abeona.HandleMerchantView)
```

Unit test yang dibuat oleh penulis pada kedua endpoint tersebut adalah sebagai berikut.
```yaml
func TestHandleProductView(t *testing.T) {
	testFibs := map[string]ProductFib{
		"Success": ProductFib{
			ReqURI:  "/product",
			ResCode: http.StatusOK,
			ResBody: "{\"products\":[{\"product_id\":1234,\"other\":\"foobar\"},{\"product_id\":1235,\"other\":\"foobar\"}]}",
		},
		"Bad Json Marshal": ProductFib{
			PatchJSONMarshal: mockJSONMarshalFail,
			ReqURI:           "/product",
			ResCode:          http.StatusBadRequest,
			ResBody:          "{\"err\":{}}",
		},
	}

	for _, fib := range testFibs {
		// monkeypatch
		if fib.PatchJSONMarshal != nil {
			jsonMarshal = fib.PatchJSONMarshal
		}

		r := gofight.New()
		r.GET(fib.ReqURI).
			Run(GinEngine(), func(res gofight.HTTPResponse, req gofight.HTTPRequest) {
				assert.Equal(t, fib.ResCode, res.Code)
				assert.Equal(t, fib.ResBody, res.Body.String())
			})

		// revert monkeypatch
		jsonMarshal = json.Marshal
	}
}

func TestHandleMerchantView(t *testing.T) {
	testFibs := map[string]MerchantFib{
		"Success": MerchantFib{
			ReqURI:  "/merchant",
			ResCode: http.StatusOK,
			ResBody: "{\"merchants\":[{\"merchant_id\":1234,\"other\":\"foobar\"},{\"merchant_id\":1235,\"other\":\"foobar\"}]}",
		},
		"Bad Json Marshal": MerchantFib{
			PatchJSONMarshal: mockJSONMarshalFail,
			ReqURI:           "/merchant",
			ResCode:          http.StatusBadRequest,
			ResBody:          "{\"err\":{}}",
		},
	}

	for _, fib := range testFibs {
		// monkeypatch
		if fib.PatchJSONMarshal != nil {
			jsonMarshal = fib.PatchJSONMarshal
		}

		r := gofight.New()
		r.GET(fib.ReqURI).
			Run(GinEngine(), func(res gofight.HTTPResponse, req gofight.HTTPRequest) {
				assert.Equal(t, fib.ResCode, res.Code)
				assert.Equal(t, fib.ResBody, res.Body.String())
			})

		// revert monkeypatch
		jsonMarshal = json.Marshal
	}
}
```

Terakhir, implementasi dari fungsi pada endpoint tersebut adalah sebagai berikut.
```yaml
//HandleProductView : Handle function for endpoint product
func HandleProductView(c *gin.Context) {
	// TODO get data from DB

	// Dummy data
	productList := map[string][]product_obj.ProductObject{
		"products": []product_obj.ProductObject{
			product_obj.ProductObject{
				ProductId: 1234,
				Other:     "foobar",
			},
			product_obj.ProductObject{
				ProductId: 1235,
				Other:     "foobar",
			},
		},
	}

	resp, err := jsonMarshal(productList)
	if err != nil {
		c.JSON(http.StatusBadRequest, gin.H{
			"err": err,
		})
		return
	}

	c.String(http.StatusOK, string(resp))
}

//HandleMerchantView : Handle function for endpoint merchant
func HandleMerchantView(c *gin.Context) {
	// TODO get data from DB

	// Dummy data
	merchantList := map[string][]merchant_obj.MerchantObject{
		"merchants": []merchant_obj.MerchantObject{
			merchant_obj.MerchantObject{
				MerchantId: 1234,
				Other:      "foobar",
			},
			merchant_obj.MerchantObject{
				MerchantId: 1235,
				Other:      "foobar",
			},
		},
	}

	resp, err := jsonMarshal(merchantList)
	if err != nil {
		c.JSON(http.StatusBadRequest, gin.H{
			"err": err,
		})
		return
	}

	c.String(http.StatusOK, string(resp))
}
```

#### To Do Next
- Menyelesaikan implementasi dengan menggunakan data dari database
- Mengambil task untuk implementasi endpoint lainnya

Referensi:
[Agile Software Development](https://www.agilealliance.org/agile101/)
[Test Driven Development](https://www.agilealliance.org/glossary/tdd/)