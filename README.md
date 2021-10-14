# oauth2-dexidp
An example docker service composition for Oauth2 flow with oauth2-proxy and dex

This is a docker composition for oauth2 flow with [oauth2-proxy](https://github.com/oauth2-proxy/oauth2-proxy). [oauth2-proxy](https://github.com/oauth2-proxy/oauth2-proxy) behaves as oauth2 client and [dex](https://github.com/dexidp/dex) is authorization service.  
Application that is protected by the proxy is [nginx-echo-headers](https://github.com/brndnmtthws/nginx-echo-headers), which serves just as an neat example since it can echoo all the headers that [oauth2-proxy](https://github.com/oauth2-proxy/oauth2-proxy) has sent.  
When authentication flow is completed [oauth2-proxy](https://github.com/oauth2-proxy/oauth2-proxy) should forward to the backend service.
Backend service echoes all the headers sent from `oauth2-proxy`.

run 
```
docker-compose up
```

try to access `http://localhost/echo`

You will be redirected to [dex](https://github.com/dexidp/dex), dex is configured as in basic example so only `admin@example.com` with `password` is valid user.  
After you enter a successfull username and password `oauth2-proxy` will redirect back to the initially requested resource.  
As a result you can see the following output in browser:
```
GET /echo HTTP/1.0
X-User: CiQwOGE4Njg0Yi1kYjg4LTRiNzMtOTBhOS0zY2QxNjYxZjU0NjYSBWxvY2Fs
X-Email: admin@example.com
X-Access-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6IjQzODczMWQ2ZTIxNGEyOGQ3Y2Y4NjdjMmYzMGYyN2VmOTNhZjFjZDUifQ.eyJpc3MiOiJodHRwOi8vbG9jYWxob3N0L2RleCIsInN1YiI6IkNpUXdPR0U0TmpnMFlpMWtZamc0TFRSaU56TXRPVEJoT1MwelkyUXhOall4WmpVME5qWVNCV3h2WTJGcyIsImF1ZCI6Im9hdXRoMi1wcm94eSIsImV4cCI6MTYzNDMwNzAyMCwiaWF0IjoxNjM0MjIwNjIwLCJhdF9oYXNoIjoiY0ZBQzdpRGZzQnQzOS1hb0U5YXNlZyIsImVtYWlsIjoiYWRtaW5ZXhhbXBsZS5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwibmFtZSI6ImFkbWluIn0.pra5RJFeMorlYySBK00eG1EB6SCSZaaIvwjRsstnrjBjVxP02CLM96TjmSMEhwqSGnUHoK_5_Cj_9JFu_FtDkidJVwc13ON0AsAoWKOxAYIVPsN0p52tfs8uCt8tdsHHZpZAlC8timCwCGFL57M7q_b4cdUsgYn6KfkxDy8tx5ZX8AmJHbiE09sjmKIduRd8HVIcaBk0HGxQpOeBilVdYeOlZIctlN255mHJf1lYDtx6ry4AFyq_CzulRtgCBkf2ubiGCLYMpayV802mf-fZFnaByk5u98zowBifBbcXJOWOPtj3REcN9eKXclu41bUubzKV7z2gSwzpDTi3S8Gpyw
Host: localhost:8080
Connection: close
sec-ch-ua: "Chromium";v="94", "Google Chrome";v="94", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.81 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: _oauth2_proxy=VeubQF-GIMQdslfw6NG7wyYgyWwks4oR37QzRC47ycJqCXfNAHbPnFyOSIAL-T4T59pl9UIkkda7spNSFJ5z0d7s1ceBHg1ZbIObY5Fk7VwX1YHY1MD-fbjgl49POmU8SjaZXNAPYKiEmiQdgTckUJruCI0ZIUTghqPokoO3bZfxdFvnLjfE16aJF_S5q0FHogb2rZtngKOnQ7Mb6jqocH02YDXbVsSVEFpCSMhugIWrCsdg-qGBSjgF8D2iOxrsL8tOCHSgqCRodvYAw_aau5cZOob2lL7fivP6B5lZ3L6PMZry3dme3YnsbZvD5H8PBCYcJL8AxkzxajHbP3eRvVbPKDbcQcdbkUS-wrR60xN51ypnzeZ0U3-Biic861hjmqx-Lotd5OPNpjZWzK3DIkspuJrgOYHfe9uBU5mIKftupSt8HhA7GYt_BsuNVCnn28cYrc5IlrcYerlQJBUeRlgRCMqtXE9a9pSR570uCiitAT9IMBzSJIvSd_9IGO7fChxrtRBnecP13__qRsgmGIhDTZjrbvVtqMewyG4VCh_hVYAd73EGPV0DBvnzgkxE3xc6LTOBIAel2yyZtcneieHq__X50PHR8u-6UjE6VNOahlnofvWCSsdPTn88p92QeFXwmTbm5NxlRV88K87z0WTbBNLDO7Og7zcDafx3pQobUjDFmBcHPXrwq3WBny_T9zWRUpwJh3FTeu2A8PRcv3n0PAaEZb7VVRb6XFumE_4eVXlMkHUsQrb1t7ULtoh4VlQWNknw1s6GEGpoXGLGV1CD7mOkfGD8lvDM6Az8ac2T-JUowMOlV9QdE4Ni6sJKS4fk0qxnxLEKM57vI-IQ5OaZGBOoWqDXxLQSeaWLikGcZkA-STzSlPor3G-_d-dU5nmY_Cn729691GWnykDr4f2DVF1u8l_4Ejt-Mq3gCn5vp54j79NWkuDHuXcZVRgDdwZDZ6RRmUFs1tOwcZu3Jhndc_z2BK9ZPoL55RpUx8JwdWpDQ46JpyjMa35c49Q77efwuGkZqicxiDeMOFKH1HBlQG7OMKskxutRuA1B7e5zXi0kyo21i9WOixXfjK34aCZ0FK0DuCttRCsR8QyGpVveswekmIDFaVg4eoIlQHW8koGXYHFmsIAAkvFh5CgqeKElqj80FhU4elLPI1KXA1axakdb332liXGTbMhePC33hNwCFTB-OzWtNi6tkxrLlg8DXWtXb20OKwuaEmBPBluO-7ROKqkSLW5ssRGq3ZtFt00BKMg_W2qn2aYbIzgESnyBcD6LmYhjkqMJJq1K2bLACivuF5WZweahkPLTYac0Ii2WnRYBfQio7iVsG_usx8SoPZbVXuFj22SZW-AzQxEenI-AlK3NxHmxFJsziDiqWkK5rv6FchNOINY_cZcHPGK97IW8z73Xgh7BvLqJb5RHLGcNCKKIAb3YLeXpfWLjuVlP2b01bUN4EH9FMBiVUB_vqPaqlj9K6U56RqSCbCMbTqx1rBV9q-leH9PrDfOgEXRz5SxXAFAOklhIIYXgUK6YW5rLCLtThQRw9pSaaDQD2Y05621QV6xenlsLjY3wPT9Exc3D-1bap8f_ystfr3DiCgwu7uGI0iZpng43pWNWlS5cVVNn4Iyuqv0sBFwVwMe36MyEz9sXoNeAhlM1oaTRRT11RJIrPqMSOl1MfsPXWlvOIFgywLiHcpRpHoTrTG1ibdK168OPNi9_jR2ByydAGyyRyLM-mE2zybrPscfnO0D_tdcVk_P5gyPXMwO5tX0IngJfYLSrgVVWwvMoJk_6hcqDxPvkHE-_spPtzT1hFF4JGnymnNib0p_12oUHrAaGcnZ6AUNEsXv9akamQR2rl51MGaarFzB9cPKINTUyYhJuCsUvfaIlJ1zO63KJpV1yAPKVeIdL2sbp1UJ2sKf3kpG9py1RojMfkSV3WiEzalHTwNK2yPCyg==|1634220620|XAMNxDdDfidsDKCkfeDvoB6FaBXOYZCAPYwo0vw0dxY=


```



