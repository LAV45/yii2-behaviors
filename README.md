yii2-behaviors
==============

[![Latest Stable Version](https://poser.pugx.org/lav45/yii2-behaviors/v/stable)](https://packagist.org/packages/lav45/yii2-behaviors)
[![License](https://poser.pugx.org/lav45/yii2-behaviors/license)](https://packagist.org/packages/lav45/yii2-behaviors)
[![Total Downloads](https://poser.pugx.org/lav45/yii2-behaviors/downloads)](https://packagist.org/packages/lav45/yii2-behaviors)

This is a set of Yii2 behaviors extensions.

## Installation

The preferred way to install this extension through [composer](http://getcomposer.org/download/).

You can set the console

```
~$ composer require lav45/yii2-behaviors
```

or add

```
    "lav45/yii2-behaviors": "^0.1"
```

in ```require``` section in `composer.json` file.


## Using ReplicationBehavior

This extension will help you to copy data from one table to the remote and to maintain the relevance of the data, tracking changes in field list `attributes`.

```php
    class Page extends ActiveRecord
    {
        public function behaviors()
        {
            return [
                [
                    'class' => 'lav45\behaviors\ReplicationBehavior',
                    'relation' => 'pageReplication',
                    'attributes' => [
                        'id' => 'id',
                        'text' => 'description',
                        'created_at' => [
                            'field' => 'createdAt',
                            'value' => function (self $ownerModel) {
                                return $ownerModel->created_at;
                            }
                        ],
                        'updated_at' => [
                            'field' => 'updatedAt',
                            'value' => 'data.updated'
                        ]
                    ]
                ]
            ];
        }

        public function getPageReplication()
        {
            return $this->hasOne(PageReplication::className(), ['id' => 'id']);
        }
    
        public function getData()
        {
            return [
                'id' => $this->id,
                'updated' => $this->updated_at,
                'created' => $this->created_at,
            ];
        }
    }
```


## License

**yii2-behaviors** it is available under a BSD 3-Clause License. Detailed information can be found in the `LICENSE.md`.
