# Holistics DBT

Generate Holistics data models based on dbt `manifest.json` and `catalogs.json`.

**Supported databases:**
- BigQuery
- (more to come later)

## Install
- `pip install holistics-dbt`

## Usage
- Import the `AmlGen` and `Dbt` classes
- Provide the path to your `manifest.json` and `catalog.json` files
- Run the `AmlGen#gen_table_models` to generate the Holistics data models
- Call `to_s` on each model to get the AML text

```python
from holistics_dbt.aml_gen import AmlGen
from holistics_dbt.dbt import Dbt

dbt = Dbt(
  manifest_path='target/manifest.json',
  catalog_path='target/catalog.json'
)

aml_gen = AmlGen(dbt_artifact=dbt)

table_models = aml_gen.gen_table_models()

for table_model in table_models:
  print(table_model.to_s())
```

Output:
```
Model model_ecommerce_dbt_model_cities {
  type: 'table'
  label: ''
  description: ''
  owner: ''
  data_source_name: 'airy-berm-145910'
  table_name: '`ecommerce`.`model_cities`'

  dimension id {
    type: 'number'
    hidden: false
    definition: @sql {{ #SOURCE.id }};;
    label: 'id'
  }

  dimension name {
    type: 'text'
    hidden: false
    definition: @sql {{ #SOURCE.name }};;
    label: 'name'
  }
}
```

## Customization

The `gen_table_models` return a list of `AmlTableModel` objects which are just plain Python objects. You can see the class definition at [aml_table_model.py](https://github.com/holistics/holistics_dbt/blob/master/holistics_dbt/aml_table_model.py).

Feel free to modify the class to suit your needs, and then call `to_s` to get the AML codes.
