---
title: "Down-level Filesets on AIX"
date: 2022-11-23T18:00:00
categories: ["devops"]
tags: ["aix","ibm","scripting"]
series: ["archive"]
---

AIX Levels are named based on Release, Technology Level (TL), Service Pack (SP), and Build Sequence Identifier e.g. 7100-05-04-1914. As well as the current OS level, the oslevel command can show details of the TLs and SPs on a server along with details of any up- or down-level filesets.

You can read more in the [IBM online documentation][1] but here are the parameters we are interested in:

{{< table "table table-bordered table-striped" >}}
|Flag|Description|
|:---:|:---|
|-f|Rebuild the cache for this operation|
|-r|All specified flags are applied to technology levels|
|-s|All specified flags are applied to service packs|
|-l level|Lists filesets that are earlier than the specified level|
|-q|Lists the known technology levels or service packs with the latest at the top of the list|
{{</ table >}}

We can confirm that our server is running the latest available level by checking the following:

- If the current OS level matches the latest known level, everything is OK.
- If not but the current TL matches the latest know TL then there is an issue with the current SP.
- Otherwise, the issue is with the current TL.

The following script shows the implementation of these steps and will display all down-level filesets, if applicable:

```sh
#!/usr/bin/ksh

return_code=0

echo "Checking oslevel..."

current_oslevel=$(oslevel -fs)
latest_oslevel=$(oslevel -sq 2>/dev/null | sed -n '1p')

if [[ "${current_oslevel}" == "${latest_oslevel}" ]]; then
  echo "Current OS Level OK: ${current_oslevel}"
else
  current_tl=$(oslevel -r)
  latest_tl=$(oslevel -rq 2>/dev/null | sed -n '1p')

  if [[ "${current_tl}" == "${latest_tl}" ]]; then
    echo "Current Service Pack appears downlevel:"
    echo "  Latest SP Level:  ${latest_oslevel}"
    echo "  Current SP Level: ${current_oslevel}"
    echo
    echo "Check the following filesets:"
    oslevel -s -l ${latest_oslevel}

    return_code=1
  else
    echo "Current Technology Level appears downlevel:"
    echo "  Latest TL Level:  ${latest_tl}"
    echo "  Current TL Level: ${current_tl}"
    echo
    echo "Check the following filesets:"
    oslevel -r -l ${latest_tl}

    return_code=2
  fi
fi

exit ${return_code}
```

[1]: https://www.ibm.com/docs/en/aix/7.2?topic=o-oslevel-command
