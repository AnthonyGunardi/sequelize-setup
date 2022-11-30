# Sequelize

## Setting Awal
---
- `npm init -y`
- `npm i express ejs pg sequelize`
- `npm i -D sequelize-cli` (optional, tidak usah jika sudah install global)
- Buat file .gitignore dan isi dengan node_modules

## Membuat Database
---
- `sequelize init`
- Edit file config.json dan ganti username, password, database, dialect
- `sequelize db:create`

## Membuat Tabel & Model
---
- `sequelize model:generate --name [model name, singular & pascal case] --attributes [contoh, name:string,age:integer,joined:dateonly]`
- Tambahkan relasi di models:
``` javascript
  //1 To 1: 
  [Source].hasOne(models.[Target], {foreignKey: '[SourceId]', sourceKey: '[id(source)]'}) //(FK ada di target)
  [Source].belongsTo(models.[Target], {foreignKey: '[TargetId]', targetKey: '[id(target)]'}) //(FK ada di source)
  //1 To Many:
  [Source].hasMany(models.[Target], {foreignKey: '[SourceId]', sourceKey: '[id(source)]'}) //(FK ada di target)
  [Source].belongsTo(models.[Target], {foreignKey: '[TargetId]', targetKey: '[id(target)]'}) //(FK ada di source)
  //Many To Many:
  [Source].belongsToMany(models.[Target], {through: models.[ConjunctionModel], foreignKey: '[SourceId]', otherKey: '[TargetId]'}) //(untuk ke2 model)
```
- Tambahkan validasi & hooks di model
- `sequelize db:migrate`

## Menambahkan Kolom Tabel
---
- `sequelize migration:generate --name [nama file migration]`
- Tambahkan queryInterface method di file migration: 
  ``` javascript
  //Jika menambahkan FK:
  Up: await queryInterface.addColumn('[Table name]', '[Column name, use format: ModelnameId]', {
        type: Sequelize.INTEGER,
        references: {
          model: '[Table name]',
          key: 'id'
        },
        onUpdate: 'cascade',
        onDelete: 'cascade'
        })
  Down: await queryInterface.removeColumn('[Table name]', '[Column name]')
  //Jika menambahkan kolom normal:
  Up: await queryInterface.addColumn('[Table name]', '[Column name]', [Data type, ex: Sequelize.STRING])
  Down: await queryInterface.removeColumn('[Table name]', '[Column name]')
  ```
- `sequelize db:migrate`
- Tambahkan kolom yang baru dibuat ke Model.

## Seeding
---
- `sequelize seed:generate --name [nama file seeder]`
- Tambahkan data tabel di file seeder, termasuk kolom createdAt & updatedAt isi dengan `new Date()`. Lengkapi syntax bagian Up & Down.
- `sequelize db:seed:all` untuk seed all atau `sequelize db:seed --seed [seeder fileName]` untuk seed specific file

