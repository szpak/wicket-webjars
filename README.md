wicket-webjars
==============

Integration of webjars for Apache Wicket. 

Current build status: [![Build Status](https://buildhive.cloudbees.com/job/l0rdn1kk0n/job/wicket-webjars/badge/icon)](https://buildhive.cloudbees.com/job/l0rdn1kk0n/job/wicket-webjars/)

**wicket-webjars** dependes on [webjars](https://github.com/webjars/webjars).

Documentation:

- [Webjars Documentation](http://www.webjars.org/documentation)
- [Available Webjars](http://www.webjars.org)

Add maven dependency:

```xml
<dependency>
  <groupId>de.agilecoders.wicket.webjars</groupId>
  <artifactId>wicket-webjars</artifactId>
  <version>0.4.1</version>
</dependency>
```

Installation:

```java
    /**
     * @see org.apache.wicket.Application#init()
     */
    @Override
    public void init() {
        super.init();

        // install installs 2 default collector instances 
        // (FileAssetPathCollector(WEBJARS_PATH_PREFIX), JarAssetPathCollector)
        // and a webjars resource finder.
        WebjarsSettings settings = new WebjarsSettings();

        // register vfs collector to use webjars on jboss (you don't need to add maven dependency)
        Set<AssetPathCollector> collectors = Sets.newHashSet(settings.assetPathCollectors());
        collectors.add(new VfsJarAssetPathCollector());
        settings.assetPathCollectors(collectors.toArray(new AssetPathCollector[collectors.size()]));
        
        WicketWebjars.install(this, settings);
    }
```

Usage
=====

Add a webjars resource reference (css,js) to your IHeaderResponse:

```java
public WebjarsComponent extends Panel {

  public WebjarsComponent(String id) {
      super(id);
  }

  @Override
  public void renderHead(IHeaderResponse response) {
    super.renderHead(response);

    response.render(JavaScriptHeaderItem.forReference(new WebjarsJavaScriptResourceReference("jquery/1.8.3/jquery.js")));
  }
}
```

Add dependencies to your pom.xml:

```xml
<dependencies>
  <dependency>
      <groupId>de.agilecoders.wicket.webjars</groupId>
      <artifactId>wicket-webjars</artifactId>
  </dependency>

  <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>1.8.3</version>
  </dependency>
</dependencies>
```

It is also possible to use a resource by adding it to your html markup directly:

```html
<img src="/webjars/jquery-ui/1.9.2/css/smoothness/images/ui-icons_cd0a0a_256x240.png"/>
```

To use always recent version from your pom you have to replace the version in path with the string "current". When resource
name gets resolved this string will be replaced by recent available version in classpath. (this feature is available since 0.2.0)

```java
public WebjarsComponent extends Panel {

  public WebjarsComponent(String id) {
      super(id);
  }

  @Override
  public void renderHead(IHeaderResponse response) {
    super.renderHead(response);

    response.render(JavaScriptHeaderItem.forReference(new WebjarsJavaScriptResourceReference("jquery/current/jquery.js")));
  }
}
```

Authors
-------

[![Ohloh profile for Michael Haitz](//www.ohloh.net/accounts/235496/widgets/account_detailed.gif)](https://www.ohloh.net/accounts/235496?ref=Detailed) 

