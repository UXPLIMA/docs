---
title: Your first job
order: 104
description: Write a job that pays for breaking sand and gravel, load it, preview it and join it.
icon: sparkles
---

## 1. Write the file

Create `plugins/uxmJobs/jobs/digger.conf`. The file name is the id.

```hocon
name { default = "Digger", tr = "Kazıcı" }
description { default = "Sand, gravel and clay the world made.", tr = "Dünyanın yaptığı kum, çakıl ve kil." }
icon = "iron_shovel"

curve {
  shape         = "linear"
  base          = 100
  step          = 40
  maximum-level = 50
}

join-cost  = "0"
leave-cost = "200 + %level% * 10"

bounds {
  money      { floor = 0, ceiling = 20 }
  experience { floor = 0, ceiling = 30 }
}

actions {
  mine {
    sand    { money = "0.4 + %level% * 0.01", experience = "0.6" }
    gravel  { money = "0.5 + %level% * 0.01", experience = "0.7" }
    clay    { money = "1.5", experience = "2.0", points = "0.1", from-level = 10 }
  }
}

rewards {
  "25" {
    icon      = "diamond_shovel"
    automatic = true
    actions   = ["[message] <good>Level 25 digger!", "[give-money] points 100"]
  }
}
```

## 2. Load and preview

```
/uxmjobs reload
/uxmjobs preview digger
```

The preview prints each row at levels 1, 10, 50 and 100 and says where a bound holds it. A broken expression refuses the
reload and names the file and the key.

## 3. Join and work

```
/jobs join digger
```

Break sand. The action bar shows what it paid. `/jobs why` explains the last wage, and `/jobs stats digger` shows the
level and what is left to the next.

Related: [Jobs and prices](../features/jobs-and-prices.md), [Action types](../features/action-types.md)
