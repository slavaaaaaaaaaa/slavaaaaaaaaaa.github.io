# [I am Slava](mailto:me@slava.lol?Subject=beer%20time) ![](https://github.com/slavaaaaaaaaaa/smaslennikov.github.io/actions/workflows/pages/pages-build-deployment/badge.svg)

Check out my [resume](docs/resume.pdf)!

## Blog posts

<table class="posts">
{% for post in site.posts %}
  <tr>
  {% if post.tag == "blag" %}
    <th><li><a href="{{ post.url }}">{{ post.title }}</a></li></th>
    <th>{{ post.date | date: "%m/%d/%Y" }}</th>
  {% endif %}
  </tr>
{% endfor %}
</table>

## Notable projects

* A replacement [controller for my Litter Robot 3](https://litter-controller.slava.lol/) written in C
* I define frequent commands as [`make` target](https://github.com/slavaaaaaaaaaa/include.mk) includes
* A recent [GnuPG](https://github.com/slavaaaaaaaaaa/packages) version packaged for CentOS and Debian
* Not that great of a script for [migrating off PostgreSQL BDR](https://github.com/slavaaaaaaaaaa/smaslennikov.github.io/blob/master/bin/migrate_bdr_to_postgres.sh)
* A [devops clock](https://slava.lol/whattimeisitrightmeow/), and [another](https://slava.lol/whattravisisitrightmeow/)
* Websites
    * [A chess club](https://chessand.beer) I frequent
    * [The Rainier fan club](https://rainier.beer)
    * [My cat](https://devopscat.com)

## Resources

* [Recipes](recipes)
* Some [haikus and rhymes](haikus)
* [Beer and kombucha labels](beers)
* My ongoing [book list](books)
* My [garage listing](garage)
* Some [cat pictures](https://devopscat.com/selfies)
* In emergency, [break open](in_emergency)

## City guides

<table class="posts">
{% for post in site.posts %}
  <tr>
  {% if post.tag == "guide" %}
    <th><li><a href="{{ post.url }}">{{ post.title }}</a></li></th>
  {% endif %}
  </tr>
{% endfor %}
</table>

## Presentations

* Another presentation [about secret storage](https://slava.lol/sada-beer-and-learn-1/) given at SADA
* Grindr birthed
    * a presentation [about secret storage](grindr-demo-day-1) as well as
    * one about a [failed POC of GoCD](grindr-demo-day-2)
* At `NordstromRack.com|Hautelook` I produced [a presentation on a few CISSP chapters](cissp-access-mgmt-presentation/) with a partner
    * though it's not as useful without our [notes](https://github.com/slavaaaaaaaaaa/cissp-access-mgmt-presentation/blob/master/presentation.md)

## [Folks and friends](https://github.com/slavaaaaaaaaaa/slavaaaaaaaaaa.github.io/blob/master/ansible/roles/dotfiles/files/.newsboat/urls)

Sorted by most updated:

* Ian Foster challenges defaults [in his regular blog](https://lanrat.com/)
* Jonathan M. Tran writes about [linux and photography](https://blog.jonathanmtran.com/)
* Ian Eusebio is documenting his knowledge in [regular blog posts](https://iangge.github.io/)
* John Paul Hayes II has a pretty cool [personal site and home API](https://jph2.net)
* Bryce Case the [musician](https://ytcracker.com)
* James Khang [blogs](https://medium.com/@jahmezz) and used to build a [game](https://20minutesadayblog.wordpress.com)
* Vetsin does [infosec stuff](http://0x.c0ffee.me/)
* Naftuli Kay writes about [engineering](https://naftuli.wtf)
* [Javelang](https://javelang.com/) is the future of programming