---
title: Hyperparameter tuning magic
date: 2019-06-20 20:06:52
---

```
def build_model(hd):
    #Define a function that retures a compiled model.
    #The functions gets a 'hp' argument which is used
    #to query hyperparameter, such as 
    #'hp.Range('num_layers',2,8)'
    inputs = keras.Input(shape=(28,28,1))
    x =inputs
    for i in range(hp.Range('num_layers',2,8)):
        x = layers.Conv2D(
            filters = hp.range('units_'+str(i),32,256
                                step=32
                                default=64),
            kernel_size = 32,
            activation='relu'
        )(x)
        pool = hp.Choice('pool_'+ str(i),[None,'max','avg'])
        if pool == 'max':
            x = layers.maxPooling2D(2)(x)
        elif pool == 'avg':
            x = layers.avgPooling2d(2)(x)
    x = layers.Flanten()(x)
    output =layers.Dense(10, activation='softmax')(x)
    model = keras.Model(inputs,outputs)
    model.compile(
        optimizer = keras.optimizers.Adam(
            learning_rate = hp.Choice('learning_rate',[1e-3,5e-4])
        ),
        loss = 'sparse_categorical_crossentropy',
        metrics = ['accuracy']
    )
    return model
# Initialize the tuner by passing the 'build_model' function
# and specifying key search constraints: maximize val_acc (objective)
# and spend 40 trails doing the search
tuner = Hyperband(
    build_model,
    objective='val_accuracy',
    max_trails = 40,
    directory = 'test_directory'
)
#Display search space overview
tuner.search_space_summary()
# Perform the model search. The search functions has
# the same singature as 'model.fit()'
tuner.search(x_train,y_train,
            batch_size=128,
            epochs= 20,
            validation_data=(x_val,y_val),
            callbacks=[
                keras.callbacks.EarlyStoping(
                    monitor ='val_accuracy',patience =1
                )
            ])
#show the best model, their Hyperparameter, and the resulting metrics.
```