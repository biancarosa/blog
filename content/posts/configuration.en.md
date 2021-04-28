+++
title = "a (fairly) clean and easy way to get configuration from multiple sources"
description = ""
tags = [
    "go",
    "development",
    "configuration-management"
]
date = "2021-04-27"
categories = [
    "development"
]
author = "@__biancarosa"
+++

Configuration management is such a fun problem to solve! You can use libraries like [dynaconf](https://www.dynaconf.com/) (Python) or [viper](https://github.com/spf13/viper) (Go) and they're super fancy and will probably provide everything you want (and a bit more).

Today I wanted to code, though. For a pet project, I wanted something as simple as: well, try to get this configuration from env vars, and try to look in a secret manager for those you can't find. I wanted no dependencies for this small task, for this small project, and for those starting with configuration management, it might be a fun way to understand the power those libraries can give you.

In this particular project, I was using Go!

So at first, I made sure I had a structure defining what I needed for my app.

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
type Config struct {
	NonSecretVariable                   string
	SecretVariable                      string
	VeryImportantSecretVariable         string
}
{{< / highlight >}}

This is a struct that should know how to validate itself. It returns an error if it has a value that is null (or invalid, if you wanna check that, be my guest) on a very important value.

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
func (c *Config) Validate() error {
	if c.VeryImportantSecretVariable == "" {
		return errors.New("VeryImportantSecretVariable must be provided")
	}
	return nil
}
{{< / highlight >}}

Then, to populate this structure, I had a chunk of code that ended up pretty elegant:

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
func GetConfig() *Config {
	c := new(Config)
	ep := new(EnvironmentVariablesConfigurationProvider)
	c = ep.PopulateConfig(c)
	if err := c.Validate(); err == nil {
		return c
	}

	gsmp := new(GoogleSecretManagerConfigurationProvider)
	c = gsmp.PopulateConfig(c)
	if err := c.Validate(); err != nil {
		panic(err)
	}
	return c
}
{{< / highlight >}}

In the above code I access two implementations of a *ConfigurationProvider* interface. If the first one doesn't give me all variables I want, then I go for the second one. In my case, I was using [Google Secret Manager](https://cloud.google.com/secret-manager) to store a secret, but you can use [Azure Key Vault](https://azure.microsoft.com/pt-br/services/key-vault/), Hashicorp's [Vault](https://www.hashicorp.com/products/vault), or, y'know, pretty much anything that floats your boat.

The implementation of the providers is not gonna be covered here because they can be pretty length (and in all honesty, I don't wanna code a configuration management library), but I'd like to bring up the importance of storing your secrets more securely and show how easy it can be to achieve that without having to add a external dependency.

That's all for now! :D