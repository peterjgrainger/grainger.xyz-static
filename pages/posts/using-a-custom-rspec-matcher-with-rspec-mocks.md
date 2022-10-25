---
  type: posts
  title: Using a Custom RSpec Matcher with RSpec mocks
  date: 2019-01-27
---
  
I wanted to check whether a value has been passed to a mock in my code, the solution seems simple using the `with` method.  I have isolated the bit I am interested in to reduce the noise.

```
def save!(document)
    service.save(document)
end
```

For the test I would normally I would use the standard `with` method and pass the expected value.

```
expect(mock).to_receive(:save!).with(expected_document)
```

However my expected object contains a Date.  As the `with` method calls the `==` the comparisson fails.

The [documentation](http://rspec.info/documentation/3.4/rspec-mocks/#label-Expecting+Arguments) includes lots of examples I *could* use like `kind_of(Class)` or `anything()` but then I'm not testing much.

## Custom Matchers.

As the `with` method accepts any RSpec Matcher, a custom matcher solves the problem.

At the top of the file add a custom matcher that checks the `document_id` is the same in the actual and expected object and that the created and updated times are within the last 60 seconds.

```
RSpec::Matchers.define :other_document do |expected|
  match do |actual|
    actual.user_id == expected.user_id &&
      actual.document_id == expected.document_id &&
      actual.created.between?(Time.now - 60, Time.now)
      actual.updated.between?(Time.now - 60, Time.now)
  end
end
```

We can then use the custom matcher in our mock expectation

```
expect(mock).to_receive(:save!).with(other_document(expected_document))
```

Which then passes ✅