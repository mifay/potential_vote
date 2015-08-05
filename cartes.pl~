#!/usr/bin/perl

# Copyright 2015 Michael Fayad
#
# This file is part of potential_vote.
#
# This file is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This file is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this file.  If not, see <http://www.gnu.org/licenses/>.

use strict;
use warnings;

open LecteurDeFichier,"<input.txt" or die "1E/S : $!\n";
open LecteurKmlHead,"<KmlHead.txt" or die "2E/S : $!\n";
open LecteurKmlStyles,"<KmlStyles.txt" or die "3E/S : $!\n";
open LecteurKmlFoot,"<KmlFoot.txt" or die "4E/S : $!\n";
open RedacteurKML,">output.kml" or die $!;
open RedacteurDeFichier,">output.txt" or die $!;

while (my $Ligne = <LecteurKmlHead>)
{
   print RedacteurKML "$Ligne";
}

while (my $Ligne = <LecteurKmlStyles>)
{
   print RedacteurKML "$Ligne";
}

# Styles KML
my $Pourcentage0a0    = "pourcentage0a4_principal";
my $Pourcentage1a2    = "pourcentage5a9_principal";
my $Pourcentage3a5  = "pourcentage10a14_principal";
my $Pourcentage6a8  = "pourcentage15a19_principal";
my $Pourcentage9a11  = "pourcentage20a24_principal";
my $Pourcentage12a14  = "pourcentage25a29_principal";
my $Pourcentage15a17  = "pourcentage30a34_principal";
my $Pourcentage18a20  = "pourcentage35a39_principal";
my $Pourcentage21plus = "pourcentage40plus_principal";

my $PqVoteIdx = 3;
my $PlqVoteIdx = 2;
my $CaqVoteIdx = 6;
my $QsVoteIdx = 7;
my $OnVoteIdx = 0;

my $PourcentageVotesPQ = 0;
my $PourcentageVotesPLQ = 0;
my $PourcentageVotesCAQ = 0;
my $PourcentageVotesQS = 0;
my $PourcentageVotesON = 0;
my $PourcentageVotesAucun = 0;

my $VotesPQ = 0;

while (my $Ligne = <LecteurDeFichier>)
{
   if($Ligne =~ /^(.+);(\d+\.\d+);(\d+);(.+);(\d+);(\d+);(\d+);(\d+);(\d+);(\d+);(\d+);(\d+)/)
   {
      my @VoteArr = ($5,$6,$7,$8,$9,$10,$11,$12);
      my $SectionId = $3;
      my $TotalVotes = 0;
      my $NbElecteursInscrits = $4;
      for(my $i = 0; $i < 8; ++$i)
      {
         $TotalVotes += $VoteArr[$i];
      }
      
      if($TotalVotes ne 0)
      {      
         $PourcentageVotesPQ = $VoteArr[$PqVoteIdx]/$NbElecteursInscrits;
         $PourcentageVotesPLQ = $VoteArr[$PlqVoteIdx]/$NbElecteursInscrits;
         $PourcentageVotesCAQ = $VoteArr[$CaqVoteIdx]/$NbElecteursInscrits;
         $PourcentageVotesQS = $VoteArr[$QsVoteIdx]/$NbElecteursInscrits;
         $PourcentageVotesON = $VoteArr[$OnVoteIdx]/$NbElecteursInscrits;
	 $PourcentageVotesAucun = 1-($TotalVotes/$NbElecteursInscrits);

	 $VotesPQ = $VoteArr[$PqVoteIdx]/$TotalVotes;	 
      }
      else
      {
         $PourcentageVotesPQ = 0;
         $PourcentageVotesPLQ = 0;
         $PourcentageVotesCAQ = 0;
         $PourcentageVotesQS = 0;
         $PourcentageVotesON = 0;
	 $PourcentageVotesAucun = 0;

	 $VotesPQ = 0;
      }
      
      my $PourcentageVotes = $PourcentageVotesAucun * $VotesPQ;      

      open LecteurGPS,"<GpsSv.txt" or die "E/S : $!\n";
      
      while (my $Ligne = <LecteurGPS>)
      {
         if($Ligne =~ /^$SectionId,\t\t\t\t\t\t\t\t\t(.+)$/)
         {
            my $StyleToUse = $Pourcentage0a0;
            if ($PourcentageVotes < 0.00)
            {
               $StyleToUse = $Pourcentage0a0;
            }
            elsif ($PourcentageVotes < 0.02)
            {
               $StyleToUse = $Pourcentage1a2;
            }                  
            elsif ($PourcentageVotes < 0.05)
            {
               $StyleToUse = $Pourcentage3a5;
            }     
            elsif ($PourcentageVotes < 0.08)
            {
               $StyleToUse = $Pourcentage6a8;
            }     
            elsif ($PourcentageVotes < 0.11)
            {
               $StyleToUse = $Pourcentage9a11;
            }     
            elsif ($PourcentageVotes < 0.14)
            {
               $StyleToUse = $Pourcentage12a14;
            }     
            elsif ($PourcentageVotes < 0.17)
            {
               $StyleToUse = $Pourcentage15a17;
            }     
            elsif ($PourcentageVotes < 0.20)
            {
               $StyleToUse = $Pourcentage18a20;
            }  
            else
            {
               $StyleToUse = $Pourcentage21plus;
            }                       
            
            my $PourcentageVotesStrPq = sprintf("%.3f", $PourcentageVotesPQ);
            my $PourcentageVotesStrPlq = sprintf("%.3f", $PourcentageVotesPLQ);
            my $PourcentageVotesStrCaq = sprintf("%.3f", $PourcentageVotesCAQ);
            my $PourcentageVotesStrQs = sprintf("%.3f", $PourcentageVotesQS);
            my $PourcentageVotesStrOn = sprintf("%.3f", $PourcentageVotesON);
	    my $PourcentageVotesStrAucun = sprintf("%.3f", $PourcentageVotesAucun);
	    my $Potentiel = sprintf("%.3f", $PourcentageVotes);
            
            print RedacteurKML "\t<Placemark><name>$SectionId</name><description>Indice de $Potentiel\n$PourcentageVotesStrPq pour le PQ\n$PourcentageVotesStrPlq pour le PLQ\n$PourcentageVotesStrCaq pour la CAQ\n$PourcentageVotesStrQs pour QS\n$PourcentageVotesStrOn pour ON\n$PourcentageVotesStrAucun pour aucun parti</description><styleUrl>#$StyleToUse</styleUrl><Polygon><tessellate>1</tessellate><outerBoundaryIs><LinearRing><coordinates>$1</coordinates></LinearRing></outerBoundaryIs></Polygon></Placemark>\n";
         }
      }
      
      $SectionId++;
      
      close LecteurGPS;
      print RedacteurDeFichier "$SectionId,$PourcentageVotes\n";
   }
}

while (my $Ligne = <LecteurKmlFoot>)
{
   print RedacteurKML "$Ligne";
}

close LecteurKmlHead;
close LecteurKmlStyles;
close LecteurKmlFoot;
close RedacteurKML;
close LecteurDeFichier;
close RedacteurDeFichier;
