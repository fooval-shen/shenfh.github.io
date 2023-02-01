---
layout: post
title: Spring cloud sleuth customizer
author:  fooval
catalog: true
date:  2023-02-01 08:00:00
tags:
    - Spring
header-img: img/blog-0.jpg
---


## Spring cloud sleuth customizer

```Kotlin
## SleuthConfig.kt

    /**
     * https://docs.spring.io/spring-cloud-sleuth/docs/current/reference/html/howto.html#how-to-add-headers-to-the-http-server-response
     */
    @Bean
    fun traceFilter(tracer: Tracer): Filter {
        return Filter { request, response, chain ->
            if (request is HttpServletRequest) {
                val username: String? = usernameField().value
                if (username == null) {
                    usernameField().updateValue("testname")
                }
            }
            val currentSpan: Span? = tracer.currentSpan()
            if (currentSpan != null) {
                val resp = (response as? HttpServletResponse)
                resp?.addHeader("Test-Trace-Id", currentSpan.context().traceId())
                resp?.addHeader("Test-Username", usernameField().value ?: "")
            }
            chain.doFilter(request, response)
        }
    }

    @Bean(name = ["username"])
    fun usernameField(): BaggageField {
        return BaggageField.create("username")
    }

    @Bean
    fun correlationScope(): CorrelationScopeCustomizer {
        return CorrelationScopeCustomizer {
            it.add(SingleCorrelationField.newBuilder(usernameField()).flushOnUpdate().build())
        }
    }

    @Bean
    fun baggagePropagation(): BaggagePropagationCustomizer {
        return BaggagePropagationCustomizer {
            it.add(BaggagePropagationConfig.SingleBaggageField.remote(usernameField()))
        }
    }
```

