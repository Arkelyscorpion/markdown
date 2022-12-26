# This is a Simple tutorial on how to use Markdown

Instructions to setup GraphQL in a Django Project
![GraphQL Logo](/GraphQL_Logo.svg.png)

### [Graphene Documentation](https://docs.graphene-python.org/projects/django/en/latest/)
---
**Table of Contents**
1. [Why GraphQL](#why-graphql)
1. [Graphene Installattion](#graphene-installation)
    1. Even i dont know how to do this.
1. [Creating Schemas](#cookbookschemapy)
1. Using Graphiql
---
## Why GraphQL
- Get only the data you want
- Easier to manage the endpoints

## Graphene Installation
Install graphene using the following command : `pip insstall  django_graphene`

# cookbook/schema.py

```py
import graphene
from graphene_django import DjangoObjectType

from cookbook.ingredients.models import Category, Ingredient

class CategoryType(DjangoObjectType):
    class Meta:
        model = Category
        fields = ("id", "name", "ingredients")

class IngredientType(DjangoObjectType):
    class Meta:
        model = Ingredient
        fields = ("id", "name", "notes", "category")

class Query(graphene.ObjectType):
    all_ingredients = graphene.List(IngredientType)
    category_by_name = graphene.Field(CategoryType, name=graphene.String(required=True))

    def resolve_all_ingredients(root, info):
        # We can easily optimize query count in the resolve method
        return Ingredient.objects.select_related("category").all()

    def resolve_category_by_name(root, info, name):
        try:
            return Category.objects.get(name=name)
        except Category.DoesNotExist:
            return None

schema = graphene.Schema(query=Query)
```

> Live and let live
> -- <cite> Someone</cite>

## RoadMap
- [ ] Task 1