# Verifiable Credential Profile Template

This [GitHub Repository Template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template) is designed to make profiling [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/) for specific use cases as easy as possible.

## Overview

This template is designed to be served through [GitHub Pages](https://pages.github.com/), and can be further customized to support custom domain names, or themes.

The template comes preconfigured to support the following extension mechanisms:

- [`@context`](https://www.w3.org/TR/vc-data-model-2.0/#contexts)
- [credentialSchema](https://www.w3.org/TR/vc-data-model-2.0/#data-schemas)
- [credentialStatus](https://www.w3.org/TR/vc-data-model-2.0/#status) 

Conformance tests are provided to ensure the profile remains valid as it is updated to support specific industry domains.

Example data is provided to convey intended use, and to support any hosted examples the profile might require for the sake of clarity to implementers.

### Setting Up GitHub Pages

Make sure to enable GitHub Pages, on the main branch serving the `/ (root)` directory.

### Custom Domain Names

The use of custom domain names is RECOMMENDED.

In addition to ensuring that your profile will remain under control of the domain controller, as opposed to a service provider.

DNS layer binding can be helpful in establishing the legitimacy of your terminology and contexts.

This is especially true for brands, constortiums or governments who control hihgly recognizable and trusted domain names on the internet.

A future update to this profile will shorten the examples provided here, and leverage the GitHub Pages support for custom domain names.

### Updating URLs

This template comes preconfigured with JSON and YAML examples that include absolute URLs and DID URLs.

You will need to perform a find replace to update the profile based on the name you choose when initializing your repository from this template:

```
OLD: transmute-industries.github.io/verifiable-credential-profile-template
NEW: <your username or organization>.github.io/<your repository name>
```

### Media Type Support

Because this profile is hosted as a static website, there is no ability to negotiate representation of resources via HTTP.
However, GitHub has special rules for serving static files, and helpfully ignores certain HTTP Headers.
Note that `application/vc+ld+json` is used to express both the core data model and data integrity proof secured verifiable credentials.
When no proof or securing mechanism is present, integrity and authentication are limited by the channel, for example HTTPS.

#### Requesting a Credential Schema

```base
curl -sL https://transmute-industries.github.io/verifiable-credential-profile-template/schemas/ExampleAlumniCredential.yaml \
-H "Accept: application/yaml"
```

#### Requesting a Credential Context

```base
curl -sL https://transmute-industries.github.io/verifiable-credential-profile-template/contexts/credentials/v1 \
-H "Accept: application/ld+json"
```

#### Requesting a Credential

```base
curl -sL https://transmute-industries.github.io/verifiable-credential-profile-template/credentials/58473 \
-H "Accept: application/vc+ld+json"
```

#### Requesting a Credential Status

```base
curl -sL https://transmute-industries.github.io/verifiable-credential-profile-template/credentials/status/8 \
-H "Accept: application/vc+ld+json"
```

### Extending a JSON-LD Context

We recommend not editing JSON-LD contexts, until you are familar with RDF processing.

Make sure all terms resolve to human readable documentation before adding them to a context.

By default, this template assumes you will use vocabulary terms defined to be understood by search engines that rely on [schema.org](https://schema.org/).

This means that [hasCertification](https://schema.org/hasCertification) and [hasGS1DigitalLink](https://schema.org/hasGS1DigitalLink) come preconfigured and supported for example:

#### Controller Document 

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://transmute-industries.github.io/verifiable-credential-profile-template/contexts/credentials/v1"
  ],
  "id": "did:web:transmute-industries.github.io:verifiable-credential-profile-template:issuers:example",
  "hasGS1DigitalLink": "https://id.gs1.org/gln/1200144791171",
  "hasCertification": {
    "type": "Certification",
    "id": "https://eprel.ec.europa.eu/qr/1779994",
    "url": "https://eprel.ec.europa.eu/screen/product/dishwashers2019/1779994"
  }
}
```

You can add additional contexts by reference or by value to the `@context`, for example:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://transmute-industries.github.io/verifiable-credential-profile-template/contexts/credentials/v1",
    "https://ref.gs1.org/gs1/vc/license-context/"
  ],
  "id": "did:web:transmute-industries.github.io:verifiable-credential-profile-template:issuers:example",
  "extendsCredential": "https://id.gs1.org/vc/license/gs1_prefix/754",
}
```

You can also add terms directly to the namespace defined for the profile.

For example you could add a new property to a controller document like this:

```json5
{
  "@context": {
    "@vocab": "http://schema.org/",
    "schema": "http://schema.org/",
    "hasGS1DigitalLink": {
      "@id": "schema:hasGS1DigitalLink",
      // @type: @id has special meaning in RDF 
      // and is not automatically added by @vocab defined terms.
      "@type": "@id"
    },

    // no need to add any schema.org terms that look like this
    // they are implicitly supported by: "@vocab": "http://schema.org/"

    // "hasCertification": {
    //   "@id": "schema:hasCertification"
    // },

    // new property added below
    "hasGLEIFWebLEI": {
      "@id": "https://example.com/hasGLEIFWebLEI",
      "@type": "@id"
    }
  }
}
```

### Cryptographic Profile Guidance

Verifiable Credentials rely on cryptography envelopes to secure specific JSON-LD media types.

Support for [JOSE](https://www.iana.org/assignments/jose/jose.xhtml#web-signature-encryption-algorithms), [Data Integrity Proofs](https://www.w3.org/TR/vc-data-integrity/), and [COSE](https://www.iana.org/assignments/cose/cose.xhtml#algorithms) in a ecosystem can limit the viability of a profile.

It is considered a best practice to require at least 1 mandatory to implement algorithm to ensure interoperability is possible.

Limiting the number of available options increases the likelyhood of interoperability, however all cryptography has a shelf life, and it is also wise to ensure that new algorithms and older algorithms are supported in an overlapping window. 

This protects against an old algorithm being broken, after years of research, and a new algorithm being broken after only a short amount of evaluation.

It is also advisable to choose algorithms that rely on different primitives, such as elliptic curves or lattices, so that a break in a category does not leave implementers of a profile with no algorithms to migrate to that are uncompromised.

### Internationalization Guidance

See the guidance from WHAT WG [URL - Living Standard](https://url.spec.whatwg.org/#idna) regarding international domain names.

You may also find the guidance from [RFC8264](https://datatracker.ietf.org/doc/rfc8264/) helpful.

There are edge cases where an internationalization issue may only become apparent after converting from JSON-LD to RDF by applying the JSON-LD `@context`.

It is recommended that profiles test all JSON-LD instances as both `application/ld+json` and `application/n-quads` (See [https://www.w3.org/TR/n-quads/](https://www.w3.org/TR/n-quads/)) for validity before finalizing a profile and publishing a context as an immutable namespace.


## Commercial Support

Commercial support for this template is available upon request from
Transmute: sales@transmute.industries