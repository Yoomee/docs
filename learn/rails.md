# So you want to learn Rails?

Like any new programming framework although it might seem like you can get
up and running very quickly without much background knowledge, spending some
time learning Ruby properly and doing some Rails example projects is
definitely worthwhile!

At Yoomee we run projects from Rails version 2(!) to 4.

We recommend learning Rails 4, as conceptually this is very similar to Rails 3
and is also what we are moving all our projects towards.

## Learn Ruby

Ruby is the basis of Rails! Understanding it will make you a better Rails
developer.

 * Ruby Koans (https://github.com/neo/ruby_koans).
 * Read the PDF book _"Agile Web Development With Rails"_ which you can download from [here](https://gitlab.yoomee.com/yoomee/docs/raw/master/learn/awdwr-internal-only-do-not-share.pdf) - it has a great chapter on Ruby.

## Learn Rails

Start with the _"Agile Web Development With Rails"_ book – it is a computer book classic, and gets you building
a project straight away. There are a lot of services like Code School and Rails
For Zombies online, these are great, but the books are better – more structured
and more coverage.

 * _"Agile Web Development With Rails"_ book which you can download from [here](https://gitlab.yoomee.com/yoomee/docs/raw/master/learn/awdwr-internal-only-do-not-share.pdf).
 * Rails Tutorial is also a great resource, and free online:
   http://www.railstutorial.org/book

## Gems

Rails code packages are called Gems.

We have our own gems (beginning ym_*) and also make a lot of use of some others.
Speak to the dev team, or look at the gem code on the [Yoomee Gitlab](https://gitlab.yoomee.com/groups/yoomee) for more info.

## Testing

Traditionally our test coverage and approach has been patchy.

We have settled on using [Cucumber](http://cukes.info/) for tests as a standard in our Rails app.
Our approach to testing is to focus on story-based integration tests, rather
than unit test every model and controller. As with everything we are completely
open to change and suggestions (backed with code, of course!).

A copy of The [Cucumber Book.pdf](https://gitlab.yoomee.com/yoomee/docs/raw/master/learn/The%20Cucumber%20Book.pdf) is in this folder, and projects like [NYA](https://gitlab.yoomee.com/nya/nya_rails4/tree/master), and
[vInspired](https://gitlab.yoomee.com/vinspired/vwidget/tree/master) have better test coverage.

## Resources

This things are useful resources that you could check out / subscribe to, which
will make more sense the more you learn.

 * thoughtbot blog - http://robots.thoughtbot.com/
 * discourse (open source project - some nice code in here, and a good example
   of best practices): https://github.com/discourse/discourse

## Help

No question is silly, or to trivial. We have all been there learning new things
and no how tough it can be. Always try and find out if you can answer your own
question first, but also be free to ask questions personally or using #dev in
[Slack](https://yoomee.slack.com/messages/dev/).
