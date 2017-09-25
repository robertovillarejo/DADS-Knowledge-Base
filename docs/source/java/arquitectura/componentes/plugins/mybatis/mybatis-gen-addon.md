# Mybatis

> **Nota:** El presente documento está basado en la documentación oficial de [MyBatis Generator](http://www.mybatis.org/generator/) y está adaptado a las necesidades de nuestros proyectos.

Los pasos para la configuración del `Generador de Mybatis` son:

1. Configuración del `pom.xml`
2. Configuración del archivo `generatorConfig.xml`
3. Comandos para generar código.

## Configuración del pom.xml

**Agregar el repositorio de plugins de la `DADS`:**

```xml
<pluginRepositories>
        <pluginRepository>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
            <id>central</id>
            <name>plugins-release</name>
            <url>http://artifactory.dads.infotec.mx/artifactory/plugins-release</url>
        </pluginRepository>
        <pluginRepository>
            <snapshots/>
            <id>snapshots</id>
            <name>plugins-snapshot</name>
            <url>http://artifactory.dads.infotec.mx/artifactory/plugins-snapshot</url>
        </pluginRepository>
</pluginRepositories>
```

**Agregar el plugin de `Mybatis Generator`**:

```xml
<!-- MyBatis Generator plugin -->
 <build>
     .
     .
	<plugins>
           .
           .
		<!-- MyBatis Generator plugin -->
		<plugin>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-maven-plugin</artifactId>
			<version>1.3.3</version>
			<configuration>
				<configurationFile>${basedir}/src/main/config/generatorConfig.xml</configurationFile>
				<outputDirectory>${basedir}</outputDirectory>
				<overwrite>true</overwrite>
			</configuration>
			<dependencies>
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>5.1.39</version>
				</dependency>
				<dependency>
					<groupId>com.microsoft.sqlserver</groupId>
					<artifactId>sqljdbc4</artifactId>
					<version>4.2-SNAPSHOT</version>
				</dependency>
                <dependency>
					<groupId>postgresql</groupId>
					<artifactId>postgresql</artifactId>
					<version>9.3-1102.jdbc4</version>
				</dependency>
				<dependency>
					<groupId>mx.infotec.dads.tools.mybatis</groupId>
					<artifactId>mybatis-addon</artifactId>
					<version>1.0.0</version>
				</dependency>
			</dependencies>
		</plugin>
        .
        .
	</plugins>
    .
    .
</build>
```

## Crear el archivo generatorConfig.xml

Crear el archivo `src/main/config/generatorConfig.xml` y agregar la configuración por defecto siguiente:

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE generatorConfiguration  
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"  
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- Generaremos las tablas para MyBatis versión 3 -->
    <context id="DB1" targetRuntime="MyBatis3" defaultModelType="conditional">
        <property name="javaFileEncoding" value="UTF-8" />

        <!-- **PLUGINS** -->
        <plugin type="org.mybatis.generator.plugins.RowBoundsPlugin" />
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin" >
            <property name="useToStringFromRoot" value="true" />
        </plugin>
        <plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin" >
            <property name="useEqualsHashCodeFromRoot" value="true" />
        </plugin>
        <plugin type="org.mybatis.generator.plugins.CaseInsensitiveLikePlugin" />
        <!-- Hace que las clase de modelo implementen la interface java.io.Serializable
        para los objetos generados por MBG -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin">
            <property name="addGWTInterface" value="false" />
            <property name="suppressJavaInterface" value="false" />
        </plugin>

        <commentGenerator
            type="mx.infotec.dads.tools.mybatis.InfotecMybatisCommentGenerator">
            <property name="suppressAllComments" value="false" />
            <property name="suppressDate" value="false" />
        </commentGenerator>

        <!--Parámetros de conexión a la bd -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost/bien"
                        userId="developer" password="temporal123">
        </jdbcConnection>

        <!-- Modelo -->
        <javaModelGenerator targetPackage="mx.gob.conacyt.bien.model"
                            targetProject="src/main/java">
            <property name="enableSubPackages" value="false" />
            <property name="constructorBased" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!-- Mapper XML -->
        <sqlMapGenerator targetPackage="myBatis.app.gen"
                         targetProject="src/main/resources">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- También podríamos indicar el tipo ANNOTATEDMAPPER -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="mx.gob.conacyt.bien.persistence"
                             targetProject="src/main/java">
            <property name="methodNameCalculator" value="EXTENDED" />
        </javaClientGenerator>

        <!-- Tablas -->
        <table tableName="area_conocimiento" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="cargo" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="cuestionario" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="cuestionario_reactivo" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="direccion" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="entity_field_lang" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="estatus_proceso" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="estatus_proyecto" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="evaluador" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="genero" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="idioma" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="instituciones" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="nivel_area_conocimiento" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="organo_eval_cuestionario" modelType="flat" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="pais" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="perfil_evaluador" modelType="flat" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="perfil_nivel_evaluador" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="persona" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="proyecto" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="evaluador_proyecto_status" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="proyecto_en_tipo" modelType="flat" >
            <property name="constructorBased" value="true" />
        </table>
        <table tableName="reactivo" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="tipo_persona" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="tipo_proyecto" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>
        <table tableName="titulo_persona" >
            <property name="constructorBased" value="true" />
            <generatedKey type="post" identity="true" column="id"
                                      sqlStatement="JDBC" />
        </table>

    </context>

    <!-- Generaremos las vistas para MyBatis versión 3 -->
    <context id="DB2" targetRuntime="MyBatis3" defaultModelType="flat">
        <property name="javaFileEncoding" value="UTF-8" />

        <!-- **PLUGINS** -->
        <plugin type="org.mybatis.generator.plugins.RowBoundsPlugin" />
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin" >
            <property name="useToStringFromRoot" value="true" />
        </plugin>
        <plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin" >
            <property name="useEqualsHashCodeFromRoot" value="true" />
        </plugin>
        <plugin type="org.mybatis.generator.plugins.CaseInsensitiveLikePlugin" />
        <!-- Hace que las clase de modelo implementen la interface java.io.Serializable
        para los objetos generados por MBG -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin">
            <property name="addGWTInterface" value="false" />
            <property name="suppressJavaInterface" value="false" />
        </plugin>

        <commentGenerator
            type="mx.infotec.dads.tools.mybatis.InfotecMybatisCommentGenerator">
            <property name="suppressAllComments" value="false" />
            <property name="suppressDate" value="false" />
        </commentGenerator>

        <!--Parámetros de conexión a la bd -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost/bien"
                        userId="developer" password="temporal123">
        </jdbcConnection>

        <!-- Modelo -->
        <javaModelGenerator targetPackage="mx.gob.conacyt.bien.model.join"
                            targetProject="src/main/java">
            <property name="enableSubPackages" value="false" />
            <property name="constructorBased" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!-- Mapper XML -->
        <sqlMapGenerator targetPackage="myBatis.app.join"
                         targetProject="src/main/resources">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- También podríamos indicar el tipo ANNOTATEDMAPPER -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="mx.gob.conacyt.bien.persistence.join"
                             targetProject="src/main/java">
            <property name="methodNameCalculator" value="EXTENDED" />
        </javaClientGenerator>

        <!-- Vistas -->
        <table tableName="evaluador_info" modelType="hierarchical"
                  enableInsert="false" enableSelectByPrimaryKey="false" enableUpdateByPrimaryKey="false"
                  enableDeleteByPrimaryKey="false" enableDeleteByExample="false" enableUpdateByExample="false">
            <property name="constructorBased" value="false" />
            <columnOverride column="nombre_completo" jdbcType="VARCHAR" />
        </table>

        <table tableName="evaluador_proyecto_info" modelType="hierarchical"
                  enableInsert="false" enableSelectByPrimaryKey="false" enableUpdateByPrimaryKey="false"
                  enableDeleteByPrimaryKey="false" enableDeleteByExample="false" enableUpdateByExample="false">
            <property name="constructorBased" value="false" />
            <columnOverride column="nombre_completo" jdbcType="VARCHAR" />
        </table>

    </context>
</generatorConfiguration>
```

## Comandos para generar código

Para la generación del código se utilizará el plugin de mybatis-generator definido en los pasos anteriores, por lo tanto, para utilizarlo lo podemos hacer a través de los siguientes comandos:

```bash
mvn -Dmybatis.generator.overwrite=true mybatis-generator:generate
```
