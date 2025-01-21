# El Analysis
## ¿Cuales son las cualidades más cotizadas por los 3 puestos más populares en en el area de data?
Para encontrar las habilidades más requeridas en el mercado español, me centré en filtrar primero los 3 puestos más cotizados o requeridos en el mercado laboral. Filtré el nombre de esos 3 puestos más requeridos, y de allí, filtré las 5 habilidades más buscadas para este tipo de roles. Este query, explora los trabajos más populares junto con las habilidades más cotizadas que debería tener para tener más exito en la busqueda de trabajo para este rol en particular. 

Encuentra mi cuadernocon pasos detallados aqui:
[2_Skill_Count.ipynb](Python_data_proyect\3_Project\2_Skill_Count.ipynb)

### Visualización 

En primer lugar utilizamos una base de datos que encontramos en hugging face con datos de la base de datos 'data_jobs' que contiene una base de datos comprensiva de datos y que contiene datos para España. Limitaciones posibles como el idioma puede ser un factor importante para considerar por temas de convenciones y obtención de datos por parte del autor lo que puede que no tenga una representación del todo completa de la oferta laboral en España.


```python
import ast
import pandas as pd 
from datasets import load_dataset
import matplotlib.pyplot as plt 
import seaborn as sns 

dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
``` 

Tomé y filtre todos los datos para encontrar la información relevante en la base de datos sobre España y hice un conteo de cada habilidad requerida para los puestos en la base de datos para poder sacar el porcentaje

```python
df_skills_spain = df[df['job_country'] == 'Spain'].copy()
df_skills_spain = df_skills_spain.explode('job_skills')
skills_count = df_skills_spain.groupby(['job_skills', 'job_title_short']).size()
df_skills_count = skills_count.reset_index(name='skill_count')
df_skills_count.sort_values(by='skill_count', ascending=False, inplace=True)
```

La base de datos tiene una columna en donde categoriza el rol por un nombre más general dependiendo del rol que se busca. Entonces en vez de, por ejemplo, "Financial Data Analyst" que seria el rol Especifico, la columa lo categoriza cómo "Data Analyst". de aqui filtro por esta columna para encontrar los roles más requeridos en el mercado Español.

```python
job_titles = df_skills_count['job_title_short'].unique().tolist()
job_titles = sorted(job_titles[:3])
```

Finalmente, clacule los porcentajes del conteo de habilidades vs el conteo de numero de empleo disponibles y produje el grafico correspondiente.
### Resultados

![Resultados de Analysis con conteo](Python_data_proyect\Images\skill_count.png)

En principio, el resultado se puede ver en el conteo de manera más precisa debido a las limitaciones que tenia la base de datos sobre la información en españa. Si bien mencioné una parte de la limitación al inicio, al conducir el analisis se demuestran otras limitaciones que impactan el resultado. en concreto, los resultados recopilados en españa contienen, en su mayoria, valores nulos en la lista de habilidades por lo que los porcentajes no son representativos reales de la muestra de sampleo.

![Resultados de Analysis con porcentajes](Python_data_proyect\Images\Skills_percentage.png)

### Conclusiones

-SQL y Python son las habilidades más universales y demandadas en los tres roles. Esto subraya la importancia de estas herramientas en el ámbito de los datos.

-Las herramientas y habilidades específicas varían según el rol, lo que refleja las diferencias en las responsabilidades y necesidades de cada posición.

### Recomndaciones

Si eres un aspirante a alguno de estos roles, aprender SQL y Python es una base esencial.

Especialízate según el rol que busques:

-Analistas: Aprende herramientas de visualización (Power BI, Tableau) y mejora tus habilidades en Excel.

-Ingenieros: Profundiza en tecnologías cloud (AWS, Azure) y procesamiento distribuido (Spark).

-Científicos: Amplía tu conocimiento en análisis estadístico (R) y modelado avanzado.

Considera la relevancia de las herramientas de nube y procesamiento big data, dado que son habilidades con creciente demanda.
