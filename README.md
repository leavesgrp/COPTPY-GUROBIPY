# Python Interface: coptpy vs gurobipy


## Main features
```bash
  1. Replace gurobipy            by coptpy                    (importing pakcage)
  2. Replace GRB                 by COPT                      (for constant values)
  3. Replace name                by nameprefix                (for creating names)
  4. Replace optimize()          by solve() 
  5. Replace getAttr('X')        by getInfo(COPT.Info.Value)  (for getting solutions)
  6. Replace ConstrName/Varname  by name                      (for getting names)
```

|     |**Import Package**  |
|:--- | :---    |
|**COPT**| import `coptpy` as `cp` <br> from `coptpy` import `COPT` 
|**GUROBI**| import `gurobipy` as gp <br>  from `gurobipy` as GRB |
|     | **Create Model** |
|**COPT**   | ```env = cp.Envr()``` <br> ```model = env.createModel('sudoku')``` 
|**GUROBI**| ```model = gp.Model('sudoku')``` |
|     |**Add variables**  | 
|**COPT**  |``` vars = model.addVars(n, n, n, vtype=COPT.BINARY, nameprefix='G') ``` 
|**GUROBI**| ``` vars = model.addVars(n, n, n, vtype=GRB.BINARY, name='G') ``` |
|   |**Add constraints**|
|**COPT** | <code> model.addConstrs((</br>&nbsp;&nbsp;vars.sum(i, j, '*') == 1 </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for i in range(n) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for j in range(n)), nameprefix='V') </code> |
|**GUROBI**| <code> model.addConstrs((</br>&nbsp;&nbsp;vars.sum(i, j, '*') == 1 </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for i in range(n) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for j in range(n)), name='V') </code> |
| |**Add constraints usng quicksum** | 
|**COPT**| <code>model.addConstrs((</br>&nbsp;&nbsp;cp.quicksum(vars[i, j, v] for i in range(i0 * s, (i0+1) * s) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for j in range(j0 * s, (j0+1) * s)) == 1 </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for v in range(n) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for i0 in range(s) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for j0 in range(s)), nameprefix='Sub)</code> | 
|**GUROBI**| <code>model.addConstrs((</br>&nbsp;&nbsp;gp.quicksum(vars[i, j, v] for i in range(i0 * s, (i0+1) * s) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for j in range(j0 * s, (j0+1) * s)) == 1 </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for v in range(n) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for i0 in range(s) </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for j0 in range(s)), name='Sub)</code> |
||**Solve** | 
|**COPT**| ```model.solve()``` | 
|**GUROBI** |```model.optimize()``` |
||**Get solutions** | 
|**COPT**| ``` solution = model.getInfo(COPT.Info.Value, vars)``` |
|**GUROBI**| ```solution = model.getAttr('X', vars)``` |
||**Get names** | 
|**COPT**| ``` constraint.name``` <br> ```variable.name``` |
|**GUROBI**| ``` constraint.ConstrName``` <br> ```variable.VarName``` |
||**Parameter**<br>**Attribute**<br>**Checking status** | 
|**COPT** |```model.param.timelimit = 60``` <br> ```model.objval``` <br> ```model.status = COPT.OPTIMAL``` | 
|**GUROBI** | ```model.params.timelimit = 60``` <br> ```model.objval``` <br> ```model.status = GRB.OPTIMAL``` |
| | | |

## Other Features

|     | **Add MIP start**  |
|:--- | :---    |
|**COPT**| ```model.setMipStart(vars[i], 0.0)``` <br> ```model.loadMipStart()``` | 
|**GUROBI** |```vars[i].Start = 0.0``` |
| |**Add constraints with two bounds**| 
|**COPT** | <code>for c in c_list: </br>&nbsp;&nbsp;m.addConstr(cp.quicksum(</br>&nbsp;&nbsp; coef[i, c] * vars[f] for i in vlist) == </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [constr_lower[c], constr_upper[c]], c) </code> |
|**GUROBI** | <code>m.addConstrs(gp.quicksum(</br>&nbsp;&nbsp;coef[i, c] * vars[f] for i in vlist) == </br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[constr_lower[c], constr_upper[c]] for c in c_list, "_")</code> |
||**Linear expression** | 
|**COPT** |```expr = LinExpr(1.0)``` <br> ```expr.addTerm(x, 2.0)``` |
|**GUROBI** | ```expr = LinExpr(1.0)``` <br> ```expr.addTerm(2.0, x)``` |
| | | |