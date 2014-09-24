;;Our toy nominal lexicon (a list of noun labels)
  (define nouns (list 'dog 'cow 'stone 'steel 'farm 'house 'cake 'bread 'knife 'cup))

;;A category is the value of the function 'cat-of' applied to a noun. For other alternatives see previous week
(define cat-of (lambda (noun) (case noun
                              ((or 'dog 'cow) 'animal)
                              ((or 'house 'farm) 'location)
                              ((or 'stone 'steel) 'material)
                              ((or 'knife 'cup) 'artifact)  
                              ((or 'cake 'bread) 'edible)
       			              (else 'unknown-category)))) 

;determine N1
(define N1 (lambda (noun1 noun2) noun1))
;determine N2
(define N2 (lambda (noun1 noun2) noun2))

;Shorthand for the composition of cat-of(N1(compound)) and N2
(define cat-N1 (lambda (noun1 noun2) (cat-of (N1 noun1 noun2))))
(define cat-N2 (lambda (noun1 noun2) (cat-of (N2 noun1 noun2))))

;Relation for individual nouns depending on their position
;Relation for N1 (read as: "N2 R N1")
(define rel-N1 (lambda (noun-in-N1) (case (cat-of noun-in-N1)
                                          (('animal) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                 '(0.3 0.3 0.2 0.05 0.1 0.05)))                                                                               
                                          (('material) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                   '(0.34 0.01 0.54 0.01 0.05 0.05)))
                                          (('location)  (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                    '(0.16 0.76 0.01 0.01 0.01 0.05)))
                                          (('artifact) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                   '(0.2 0.01 0.05 0.05 0.01 0.68)))
                                          (('edible) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                 '(0.6 0.01 0.28 0.01 0.05 0.05)))      
                                          (else 'guess-a-relation))))
    ;Note for testing: In pure Scheme, replace the "then"-condition by 'sample-from-multinomial-CATEGORY-NAME


;Relation for N2 (read as: "N2 R N1")
(define rel-N2 (lambda (noun-in-N2) (case (cat-of noun-in-N2)
                                          (('animal) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                                             '(0.01 0.48 0.4 0.05 0.01 0.05)))
                                          (('artifact) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                                             '(0.34 0.1 0.55 0.05 0.01 0.05)))
                                          (('location) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                                             '(0.2 0.39 0.4 0.05 0.01 0.05)))
                                          (('material) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                                             '(0.6 0.05 0.05 0.05 0.05 0.2)))
                                          (('edible) (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                                             '(0.4 0.05 0.4 0.05 0.05 0.05)))
                                          (else 'guess-a-relation))))

    ;Note for testing: In pure Scheme, replace the "then"-condition by 'sample-from-multinomial-CATEGORY-NAME

;Shorthand for a list with two (!) relations for two nouns at the same time:
(define multi-rel (lambda (noun1 noun2) (list (rel-N1 noun1) (rel-N2 noun2))))

;Chose a relation based on the following heuristic: 
(define relation (lambda (noun1 noun2) (cond ((equal? (rel-N1 noun1) (rel-N2 noun2)) (rel-N1 noun1)) ;If both relations coincide, go with that.
                                             (else (multinomial (list 'for_purpose 'habitat_of 'made_of 'half-half 'resembles 'unknown)
                                                                                             '(0.05 0.05 0.05 0.05 0.05 0.75)))))) ;samples from all relations with a bias towards a relation not yet learned.

(hist (repeat 50 (lambda () (relation 'steel 'knife))))
