Text::LTSV
=======

[![Build Status](https://travis-ci.org/naoya/perl-Text-LTSV.png?branch=master)](https://travis-ci.org/naoya/perl-Text-LTSV) [![Coverage Status](https://coveralls.io/repos/naoya/perl-Text-LTSV/badge.png?branch=master)](https://coveralls.io/r/naoya/perl-Text-LTSV)

NAME
-----

Text::LTSV - Labeled Tab Separated Value manipulator

SYNOPSIS
--------

      use Text::LTSV;
      my $p = Text::LTSV->new;
      my $hash = $p->parse_line("hoge:foo¥tbar:baz¥n");
      is $hash->{hoge}, 'foo';
      is $hash->{bar},  'baz';

      my $data = $p->parse_file('./t/test.ltsv'); # or parse_file_utf8
      is $data->[0]->{hoge}, 'foo';
      is $data->[0]->{bar}, 'baz';

      # Iterator interface
      my $it = $p->parse_file_iter('./t/test.ltsv'); # or parse_file_iter_utf8
      while ($it->has_next) {
          my $hash = $it->next;
          ...
      }
      $it->end;

      # Only want certain fields?
      my $p = Text::LTSV->new;
      $p->want_fields('hoge');
      $p->parse_line("hoge:foo¥tbar:baz¥n");

      # Vise versa
      my $p = Text::LTSV->new;
      $p->ignore_fields('hoge');
      $p->parse_line("hoge:foo¥tbar:baz¥n");

      my $ltsv = Text::LTSV->new(
        hoge => 'foo',
        bar  => 'baz',
      );
      is $ltsv->to_s, "hoge:foo¥tbar:baz";

DESCRIPTION
-----------

Labeled Tab-separated Values (LTSV) format is a variant of Tab-separated Values (TSV). Each record in a LTSV file is represented as a single line. Each field is separated by TAB and has a label and a value. The label and the value have been separated by ':'.

cf: [ltsv.org](http://ltsv.org/)

This format is useful for log files, especially HTTP access_log.

This module provides a simple way to process LTSV-based string and  files, which converts Key-Value pair(s) of LTSV to Perl's hash reference(s).

AUTHOR
-------

Naoya Ito

LICENSE
-------

This library is free software; you can redistribute it and/or modify it under the same terms as Perl itself.

