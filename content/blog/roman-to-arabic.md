---
title: "Roman Numerals to Arabic Numbers"
date: 2020-06-07T06:00:00+01:00
draft: false
tags: ["architect","aws","devops","python","scripting"]
description: "Python for Beginners"
summary: "I am working towards my AWS System Architect - Associate certification, training online with A Cloud Guru. I started this last year but the course has been updated for the latest version of the exam."
---

I am working towards my AWS System Architect - Associate certification, training online with [A Cloud Guru][1]. I started this last year but the course has been updated for the latest version of the exam. A few new modules have been added to the Architect Learning Path, so I'm doing their 'Python for Beginners' course.

In the first part of the course, each lesson concludes with the tutor setting a couple of exercises. I'm quite pleased that my solution to the task of converting Roman numerals to Arabic numbers almost exactly matched his!

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
