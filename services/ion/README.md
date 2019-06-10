---
description: Describe the TJ Intranet platform as a whole
---

# Ion

## Ion

Ion \([https://ion.tjhsst.edu](https://ion.tjhsst.edu)\) is the next-generation Intranet system used at TJHSST. Using Python, Django, Redis, Postgres, and many other technologies, Ion was developed from the ground up to be simple, well-documented, and extensible.

Ion allows students, teachers, and staff at TJHSST to access student information, manage activity signups, and view information on news and events. [Read more about how Ion is used at Thomas Jefferson](https://ion.tjhsst.edu/about).

{% hint style="info" %}
Note: Ion is not to be referred to as "ION" in capital letters. The correct spelling is "Ion".
{% endhint %}

## History

Ion was completely student-built and was the senior researech project of James Woglom \(Class of 2016\). It was first announced on Wednesday, November 11th, 2015 concurrently with a [tribute to Iodine developers](https://web.archive.org/web/20151111231602/https://iodine.tjhsst.edu/) on the Iodine homepage. [Iodine was](https://twitter.com/TJIntranet/status/664857149324005377) [shut down](https://twitter.com/TJIntranet/status/665273342396604417) the evening of Friday November 13th, and both Iodine and Ion remained inaccessible throughout the weekend. After completing teacher training, Ion was [officially released](https://twitter.com/TJIntranet/status/666356448801251330) at the end of the school day on Monday November 16th, one day before initially planned. It ran its first Eighth Period block successfully [two days later](https://twitter.com/TJIntranet/status/666619609143975936) on Wednesday November 18th.

## Architecture

Ion is a Django application backed by a PostgreSQL database and using redis to perform in-memory caching.  

