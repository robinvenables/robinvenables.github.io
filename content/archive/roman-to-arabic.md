---
title: "Roman Numerals to Arabic Numbers"
date: 2020-06-07T06:00:00+01:00
draft: false
categories: ["DevOps"]
tags: ["python","scripting"]
series: ["archive"]
---

I am currently working towards my AWS Solutions Architect - Associate certification, studying online with [A Cloud Guru][1]. I initially started the course last year, but it has since been updated to reflect the latest exam requirements. Several new modules have been added to the Architect Learning Path, so I’m also taking their ‘Python for Beginners’ course.

In the first part of the Python course, each lesson ends with a few exercises assigned by the tutor. I'm pleased to report that my solution for converting Roman numerals to Arabic numbers closely matched the tutor's approach!

``` python3
def numerus(roman):
	values = { "i": 1, "v": 5, "x": 10, "l": 50, "c": 100, "d": 500, "m": 1000 }
	arabic = 0
	previous = 0
	for letter in roman.lower():
		current = values.get(letter, 0)
		if previous < current:
			arabic -= previous
			current -= previous
		arabic += current
		previous = current
	return arabic

```

[1]: https://acloudguru.com
