# Using Own Database Sequences

1. Create a subclass of `NumberIdWorker` bean in the `core` module and override the `getSequenceName()` method
to return a sequence declared by [the ```@IdSequence``` annotation](https://github.com/aleksey-stukalov/own-sequence/blob/master/modules/global/src/com/company/sample/annotation/IdSequence.java):

    ```java
    @Override
    protected String getSequenceName(String entityName) {
        Class<?> entityClass = AppBeans.get(Metadata.class).getSession().getClass(entityName).getJavaClass();
        if (entityClass != null && entityClass.isAnnotationPresent(IdSequence.class))
            return entityClass.getAnnotation(IdSequence.class).name();

        return super.getSequenceName(entityName);
    }
    ```
    See an example in [SampleNumberIdWorker.java](https://github.com/cuba-labs/own-sequence/blob/master/modules/core/src/com/company/sample/core/SampleNumberIdWorker.java).        

2. Register your bean in `spring.xml`.

3. Add `cuba.numberIdCacheSize = 1` to both `app.properties` and `web-app.properties`.

4. After these steps you will be able to declare sequence name as it is shown here:

    ```java
    @NamePattern("%s|name")
    @Table(name = "SAMPLE_FOO")
    @Entity(name = "sample$Foo")
    @IdSequence(name = "FOO_SEQ")
    public class Foo extends BaseLongIdEntity {
        private static final long serialVersionUID = 3829069098132933599L;

        @Column(name = "NAME")
        protected String name;
    
        public void setName(String name) {
            this.name = name;
        }
    
        public String getName() {
            return name;
        }
    }
    ```
    See an example in [Foo.java](https://github.com/aleksey-stukalov/own-sequence/blob/master/modules/global/src/com/company/sample/entity/Foo.java).        
