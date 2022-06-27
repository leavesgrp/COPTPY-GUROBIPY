# Python Interface: COPT vs GUROBI

## Prerequisite

|  | COPT 5.0.1 | GUROBI 9.5.1 |
|:---: | :---: | :---    |
|**Installation** | pip install `coptpy` | pip install `gurobipy` |

## Main Features
```bash
  1. Replace gurobipy            by coptpy                    (importing pakcage)
  2. Replace GRB                 by COPT                      (for constant values)
  3. Replace name                by nameprefix                (for creating names)
  4. Replace optimize()          by solve() 
  5. Replace getAttr('X')        by getInfo(COPT.Info.Value)  (for getting solutions)
  6. Replace ConstrName/Varname  by name                      (for getting names)
```
| **1** || **Import Package** |
|:---: | :---: | :---    |
||COPT| import `coptpy` as `cp` <br> from `coptpy` import `COPT` 
||GUROBI| import `gurobipy` as `gp` <br>  from `gurobipy` as `GRB` |
| **2** | | **Create Model** |
||COPT  | ```env = cp.Envr()``` <br> ```model = env.createModel('sudoku')``` |
||GUROBI| ```model = gp.Model('sudoku')``` |
| **2.1** | | **Create Model with Remote Server** |
||COPT  | ```config = cp.EnvConfig()``` <br> ```confit.set("cluster", "192.168.9.9")``` <br> ```env = cp.Envr(config)``` <br> ```model = env.createModel('sudoku')``` |
| **3** ||**Add Variables**  | 
||COPT  |``` vars = model.addVars(n, n, n, vtype=COPT.BINARY, nameprefix='G') ``` 
||GUROBI| ``` vars = model.addVars(n, n, n, vtype=GRB.BINARY, name='G') ``` |
| **4** |  |**Add Constraints**|
||COPT | ```model.addConstrs((vars.sum(i, j, '*') == 1``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for i in range(n)``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for j in range(n)), nameprefix='V') ``` |
||GUROBI| ```model.addConstrs((vars.sum(i, j, '*') == 1``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for i in range(n)``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for j in range(n)), name='V') ``` |
| **5** ||**Add Constraints using quicksum** | 
||COPT| ```model.addConstrs((``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```cp.quicksum(vars[i, j, v] for i in range(i0 * s, (i0 + 1) * s) for j in range(j0 * s, (j0 + 1) * s)) == 1``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for v in range(n)``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for i0 in range(s)``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for j0 in range(s)), nameprefix='Sub)``` | 
||GUROBI| ```model.addConstrs((``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```gp.quicksum(vars[i, j, v] for i in range(i0 * s, (i0 + 1) * s) for j in range(j0 * s, (j0 + 1) * s)) == 1``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for v in range(n)``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for i0 in range(s)``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```for j0 in range(s)), name='Sub)``` |
|  **6** | |**Solve** | 
||COPT| ```model.solve()``` | 
||GUROBI |```model.optimize()``` |
|  **7** |  |**Get Solutions** | 
||COPT| ``` solution = model.getInfo(COPT.Info.Value, vars)``` |
||GUROBI| ```solution = model.getAttr('X', vars)``` |
|  **8** |  |**Get Names** | 
||COPT| ``` constraint.name``` <br> ```variable.name``` |
||GUROBI| ``` constraint.ConstrName``` <br> ```variable.VarName``` |
| **9** | |**Parameter**<br>**Attribute**<br>**Checking Status** | 
||COPT |```model.param.timelimit = 60``` <br> ```model.objval``` <br> ```model.status = COPT.OPTIMAL``` | 
||GUROBI | ```model.params.timelimit = 60``` <br> ```model.objval``` <br> ```model.status = GRB.OPTIMAL``` |


## Other Features

| **1** |   | **Add MIP Start**  |
|:---: | :---: | :---    |
||COPT| ```model.setMipStart(vars[i], 0.0)``` <br> ```model.loadMipStart()``` | 
||GUROBI |```vars[i].Start = 0.0``` |
| **2** ||**Add Constraints with Two Bounds**| 
||COPT | ```for c in c_list:``` </br>&nbsp;&nbsp;&nbsp;&nbsp;```m.addConstr(cp.quicksum(``` </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```coef[i, c] * vars[f] for i in vlist) == [con_lower[c], con_upper[c]], c)``` |
||GUROBI | ```m.addConstrs(gp.quicksum(``` </br>&nbsp;&nbsp;&nbsp;&nbsp; ```coef[i, c] * vars[f] for i in vlist) == [con_lower[c], con_upper[c]] for c in c_list, "_")``` |
| **3** ||**Linear Expression** | 
||COPT |```expr = LinExpr(1.0)``` <br> ```expr.addTerm(x, 2.0)``` |
||GUROBI | ```expr = LinExpr(1.0)``` <br> ```expr.addTerm(2.0, x)``` |

