# Example DRF recursive serializer
#django #drf #python

##### Example models.py

```python
# models.py
from django.db import models

class Item(models.Model):
    id = models.AutoField(primary_key=True)
    parent = models.ForeignKey('self', null=True, blank=True, related_name='children', on_delete=models.CASCADE)
    # other fields

# serializers.py
from rest_framework import serializers

class ItemSerializer(serializers.ModelSerializer):
    children = serializers.ListSerializer(child=serializers.RecursiveField(), read_only=True)

    class Meta:
        model = Item
        fields = '__all__'
```

##### Example serializers.py

```python
# serializers.py
class RecursiveSerializer(serializers.Serializer):
    def to_representation(self, value):
        serializer = self.parent.parent.__class__(value, context=self.context)
        return serializer.data

class ItemSerializer(serializers.ModelSerializer):
    children = RecursiveSerializer(many=True, read_only=True)

    class Meta:
        model = Item
        fields = '__all__'
```
