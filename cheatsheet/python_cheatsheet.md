# Python 

## General
https://chrisalbon.com/#python
http://mathesaurus.sourceforge.net/r-numpy.html

## Datascience 8x
http://data8.org        

### Important Table Methods
http://data8.org/datascience/tables.html          
             
- Colummns: t.select(column, ...) or t.drop(column, ...)
- Rows: t.take([row, ...]) or t.exclude([row, ...])
- Sort: t.sort(column, descending=False, distinct=False)
- Condition: t.where(column, are.condition(...))
- Apply: t.apply(function, column, ...)
- Group by 1 column: t.group(column) or t.group(column, function)
- Group by columns: t.group([column, ...]) or t.group(column, function)
- Pivot Table: t.pivot(cols, rows) or t.pivot(cols, rows, vals, function)

### Joining Two Tables (inner join)      
t1.join('col_t1', 't2', 'col_t2')       
PS: Compare to R: merge(table1, table2, x=col1, y=col2)     


### Randomness
https://www.inferentialthinking.com/chapters/09/randomness.html

two_groups = make_array('treatment', 'control')     
np.random.choice(two_groups, 10)    

tosses = make_array('Tails', 'Heads', 'Tails', 'Heads', 'Heads')     
tosses == 'Heads' # array([False,  True, False,  True,  True], dtype=bool)      
np.count_nonzero(tosses == 'Heads') # 3     


## Pandas

### Select rows and columns
- https://www.shanelynn.ie/select-pandas-dataframe-rows-and-columns-using-iloc-loc-and-ix/
- https://pythonhow.com/accessing-dataframe-columns-rows-and-cells/
     
### Join, Merge, Concatenate  
- https://chrisalbon.com/python/data_wrangling/pandas_join_merge_dataframe/
- https://www.dataquest.io/blog/pandas-concatenation-tutorial/

### Apply 
- https://chrisalbon.com/python/data_wrangling/pandas_apply_operations_to_dataframes/
- http://jonathansoma.com/lede/foundations/classes/pandas%20columns%20and%20functions/apply-a-function-to-every-row-in-a-pandas-dataframe/

### Misc
- https://towardsdatascience.com/quick-dive-into-pandas-for-data-science-cc1c1a80d9c4
- https://www.analyticsvidhya.com/blog/2016/01/12-pandas-techniques-python-data-manipulation/
- https://jeffdelaney.me/blog/useful-snippets-in-pandas/

## Numpy
http://cs231n.github.io/python-numpy-tutorial/


## The Regression Line
https://www.inferentialthinking.com/chapters/15/2/Regression_Line           
         
estimate of y = r⋅x   when both variables are measured in standard units        
            
estimate of y = slope * x + intercept              
slope = r * SD of y / SD of x                
intercept = average of y - slope * average of x          
       
```
def standard_units(x):
    return (x - np.average(x))/np.std(x)

def correlation(t, x, y):
    x_su = standard_units(t.column(x))
    y_su = standard_units(t.column(y))
    return np.average(x_su * y_su)

def slope(t, x, y):
    r = correlation(t, x, y)
    return r * np.std(t.column(y))/np.std(t.column(x))

def intercept(t, x, y):
    a = slope(t, x, y)
    return np.average(t.column(y)) - a*np.average(t.column(x))

def fitted_values(t, x, y):
    a = slope(t, x, y)
    b = intercept(t, x, y)
    return a * t.column(x) + b

def residuals(t, x, y):
    return t.column(y) - fitted_values(t, x, y)

def plot_fitted(t, x, y):
    tbl = t.select(x, y)
    tbl.with_columns("Fitted values", fitted_values(t, x, y)).scatter(0)


def mse(any_slope, any_intercept):
    x = tbl.column(0)
    y = tbl.column(1)
    fitted = any_slope*x + any_intercept
    return np.mean((y - fitted) ** 2)

best = minimize(mse)


```
See also: http://data8.org/fa16/assets/final_reference_sheet.pdf



### Random Forests
https://codewords.recurse.com/issues/seven/a-tour-of-random-forests 
https://www.datascience.com/resources/notebooks/random-forest-intro
https://www.inertia7.com/projects/95
https://chrisalbon.com/machine_learning/trees_and_forests/random_forest_classifier_example/            
Video: [Nathan Epstein - Using Random Forests in Python](https://www.youtube.com/watch?v=6O4kASc-SDE)        
Video: [Gabby Shklovsky: Random Forests Best Practices for the Business World](https://www.youtube.com/watch?v=E7VLE-U07x0)        


### Machine Learning
- http://cs229.stanford.edu/      
- http://web.stanford.edu/class/cs20si/        
- https://chrisalbon.com/#articles      




