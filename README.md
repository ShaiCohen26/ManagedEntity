Demonstrates the use of source generators for tasks normally left to runtime reflection.


## `ManagedEntity` Class
```csharp
[Managed(EnableAudit = true, EnableSoftDelete = true)]
public partial class ManagedEntity
{
	[Persisted(SetOnInsert = true)]
	private int _idParent;

	[Persisted(SetOnInsert = true, SetOnUpdate = true)]
	private string _name;
}
```

## tests

### create new
```c#
ManagedEntity entityAuthority = new ManagedEntity { Id = authId, IdParent = 1};
ManagedEntity entityToCreate = new ManagedEntity { Id = Guid.NewGuid(), IdParent = 2, CreatedBy = "user1" };
entityToCreate.MapToAuthorityInsert(entityAuthority);
```	
##### *result:*
```json
{
	"Id": "2dd23b8a-a150-4038-8819-776bc16e4d9e",
	"IdParent": 2,
	"Name": null,
	"CreatedDate": "2022-12-28T15:00:22.9998815Z",
	"CreatedBy": "user1",
	"ModifiedLastDate": null,
	"ModifiedLastBy": null,
	"IsDeleted": false,
	"DeletedDate": null,
	"DeletedBy": null
}
```
### update
```c#
ManagedEntity entityToUpdate = new ManagedEntity { Id = Guid.NewGuid(), IdParent = 3, CreatedBy = "user2", ModifiedLastBy = "user2" };
entityToUpdate.MapToAuthorityUpdate(entityAuthority);
```	
##### *result:*
```json
{
	"Id": "2dd23b8a-a150-4038-8819-776bc16e4d9e",
	"IdParent": 2,
	"Name": null,
	"CreatedDate": "2022-12-28T15:00:22.9998815Z",
	"CreatedBy": "user1",
	"ModifiedLastDate": "2022-12-28T15:00:23.0009883Z",
	"ModifiedLastBy": "user2",
	"IsDeleted": false,
	"DeletedDate": null,
	"DeletedBy": null
}
```			
### soft delete
```c#
ManagedEntity entityToDelete = new ManagedEntity { Id = Guid.NewGuid(), IdParent = 4, CreatedBy = "user3", DeletedBy = "user3" };
entityToDelete.MapToAuthorityDelete(entityAuthority);
```	
##### *result:*
```json
{
	"Id": "2dd23b8a-a150-4038-8819-776bc16e4d9e",
	"IdParent": 2,
	"Name": null,
	"CreatedDate": "2022-12-28T15:00:22.9998815Z",
	"CreatedBy": "user1",
	"ModifiedLastDate": "2022-12-28T15:00:23.0009883Z",
	"ModifiedLastBy": "user2",
	"IsDeleted": true,
	"DeletedDate": "2022-12-28T15:00:23.0012889Z",
	"DeletedBy": "user3"
}
```						