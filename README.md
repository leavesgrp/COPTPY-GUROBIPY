# Python Interface: coptpy vs gurobipy


## Main features
```bash
  1. Replace gurobipy by coptpy  (importing pakcage)
  2. Replace GRB by COPT (for constant values)
  3. Replace name by nameprefix (for creating names)
  4. Replace optimize() by solve() 
  5. Replace getAttr('X') by getInfo(COPT.Info.Value) (for getting solutions)
  6. Replace ConstrName/Varname by name (for getting names)
```

|     |COPT  | GUROBI  |
|:--- | :--- | :---    |
|**Import Package**| import `coptpy` as `cp` <br> from `coptpy` import `COPT` | import `gurobipy` as gp <br>  from `gurobipy` as GRB |
|**Create model**   | ```env = cp.Envr()``` <br> ```model = env.createModel('sudoku')``` | ```model = gp.Model('sudoku')``` |
|**Add variables**  | ``` vars = model.addVars(n, n, n, vtype=COPT.BINARY, nameprefix='G') ``` | ``` vars = model.addVars(n, n, n, vtype=GRB.BINARY, name='G') ``` |
|**Add constraints**| ``` model.addConstrs((ars.sum(i, j, '*') == 1 for i in range(n) for j in range(n)), nameprefix='V') ``` | ``` model.addConstrs((ars.sum(i, j, '*') == 1 for i in range(n) for j in range(n)), `name`='V') ``` |
|**Add constraints usng quicksum** | ```model.addConstrs((cp.quicksum(vars[i, j, v] for i in range(i0*s, (i0+1)*s) for j in range(j0*s, (j0+1)*s)) == 1 for v in range(n) for i0 in range(s) for j0 in range(s)), nameprefix='Sub)``` | ```model.addConstrs((gp.quicksum(vars[i, j, v] for i in range(i0*s, (i0+1)*s) for j in range(j0*s, (j0+1)*s)) == 1 for v in range(n) for i0 in range(s) for j0 in range(s)), name='Sub)``` |
|**Solve** | ```model.solve()``` | ```model.optimize()``` |
|**Get solutions** | ``` solution = model.getInfo(COPT.Info.Value, vars)``` | ```solution = model.getAttr('X', vars)``` |
|**Get names** | ``` constraint.name``` <br> ```variable.name``` | ``` constraint.ConstrName``` <br> ```variable.VarName``` |
|**Parameter**<br>**Attribute**<br>**Checking status** | ```model.param.timelimit = 60``` <br> ```model.objval``` <br> ```model.status = COPT.OPTIMAL``` | ```model.params.timelimit = 60``` <br> ```model.objval``` <br> ```model.status = GRB.OPTIMAL``` |
| | | |

## Other Features

|     |COPT  | GUROBI  |
|:--- | :--- | :---    |
|**Add MIP start**| ```model.setMipStart(vars[i], 0.0)``` <br> ```model.loadMipStart()``` | ```vars[i].Start = 0.0``` |
|**Add constraints with two bounds**| ```for c in c_list:``` <br> ```m.addConstr(cp.quicksum(coef[i, c] * vars[f] for i in vlist) == [constr_lower[c], constr_upper[c]], c)``` | ```m.addConstrs(gp.quicksum(coef[i, c] * vars[f] for i in vlist) == [constr_lower[c], constr_upper[c]] for c in c_list, "_")``` |
|**Linear expression** | ```expr = LinExpr(1.0)``` <br> ```expr.addTerm(x, 2.0)``` | ```expr = LinExpr(1.0)``` <br> ```expr.addTerm(2.0, x)``` |
| | | |