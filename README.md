## Pydantic Cidoc-CRM Implementation
[![License](https://img.shields.io/github/license/jonasengelmann/pydantic-cidoc-crm)](LICENSE)

A Python implementation of Cidoc-CRM using pydantic and rdflib, that provides rigours type checking and safeguards against any typos.

## Usage

```python
x = E53Place(
    iri="http:/localhost/a_place",
    p1_is_identified_by=E41Appellation(
        iri="http:/localhost/a_place/appellation",
        p190_has_symbolic_content=["Berlin"]
    )
)
x.serialize()
```

```console
<http:/localhost/a_place/appellation> <http://www.cidoc-crm.org/cidoc-crm/P190_has_symbolic_content> "Berlin" .
<http:/localhost/a_place/appellation> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://www.cidoc-crm.org/cidoc-crm/E41_Appellation> .
<http:/localhost/a_place> <http://www.cidoc-crm.org/cidoc-crm/P1_is_identified_by> <http:/localhost/a_place/appellation> .
<http:/localhost/a_place> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://www.cidoc-crm.org/cidoc-crm/E53_Place>
```

### Restrictions of domain:
```python
x = E53Place(
    iri="http:/localhost/a_place",
    p48_has_preferred_identifier=E41Appellation(
        iri="http:/localhost/a_place/identifier",
        p190_has_symbolic_content="123"
    )
)
```
Will throw a validation error:
```console
pydantic.error_wrappers.ValidationError: 1 validation error for E53Place
p48_has_preferred_identifier
  Must be E42Identifier (type=value_error)
```


### Restriction of domain and safeguard against typos:

```python
x = E53Place(
    iri="http:/localhost/a_place",
    p1_is_identefied_by=E41Appellation(
        iri="http:/localhost/a_place/appellation",
        p190_has_symbolic_content="Berlin"
    )
)
```
Will throw a validation error:
```console
pydantic.error_wrappers.ValidationError: 1 validation error for E53Place
p1_is_identefied_by
  extra fields not permitted (type=value_error.extra)
```

## License

This project is licensed under MIT license - see the [LICENSE](LICENSE) file for more information.
